# Redux


Para la almacenamiento de datos que se puede convertir en una carga a largo plazo por la gran cantidad de información que se puede estar solicitando por usuario , se decidio empezar a implementar`Redux` con la inteción de bajarle la carga evitando que se hagan solicitudes de informacion duplicada

para esto los datos de las tablas y/o estaditas que el usuario puede visualiar en viajeros se estan almacenando en un stor global

de esta forma evitamos realizar peticiones para información que ya tenemos guardada temporamente.

Actualmente estamos guardando en un nuevo espacio toda aquella información que pueda tener una variación en su solicitud, como puede ser : `year`, `type` : `entry or output` ,`group`, `sede`
todo esto con la finalidad de tener la información detallada de cada una de las posibles solicitudes que se puedan repetir

<procedure title="" id="struct">
<procedure title="" id="struct-D">

Nombre de los archivos:
<procedure title="" id="struct-D-">


`XuaiiCoordi.ts`

`XuaiiGroup.ts`

`XuaiiStudents.ts`

</procedure>
    
Estos son los archivos en los cuales se guarda la información dependiendo el `Rol` o el componente.

Cada uno de ellos lleva la misma estructura.

</procedure>

    import {SET_DATA_STUDENT_XUAII} from "../actions/type";
    
`SET_DATA_STUDENT_XUAII` este es el type, se puede entender como una clave necesaria para saber a cual espacio se esta haciendo refencia y así poder guardar los datos en la estructura correspodiente


`Types` : como toda buena estructura cuenta con un tipado que ayuda a prevenir errores y tener un orden en la información requerida

    export interface StudentData {
        topic: string;
        correct_performance: number;
        incorrect_performance: number;
    }

    export interface TypeStudentFilter {
        [key:string]:[StudentData],
    }

    export interface TypeStudentId {
        [key:number]:TypeStudentFilter
    }

    export interface AppState {
        studentData: {
             [key: number]: TypeStudentId;
        };
    }   

State: Esta estructura debe contar con un estado inicial el cual nos dara la guia de que llevara el estado ya que de no ser así no podrían dar errores relacionados con
`udefined` o `null`

    export const initialState: AppState = {
        studentData: {},
        loader: false,
    };

`initialState`: este estado inicial va extendido al type 


Estructura:

    export const studentXuaiiReducer = (state = initialState, action:any)=>{
        switch (action.type) {
            case SET_DATA_STUDENT_XUAII:
                return {
                    ...state,
                    studentData: {
                        ...state.studentData,
                        [action.identifier]: {
                            ...state.studentData[action.identifier],
                            [action.year]: {
                                ...(state.studentData[action.identifier]?.[action.year] || {}),
                                [action.filter]: action.payload,
                            },
                        },
                    },
                    loader: true,
                };
            default:
            return state;
        }
    };

`studentXuaiiReducer :` tendra unos valores por default los cuales son `state`, y le `action`

`state :` este sera nuestro estado constante el cual se le asigna el estado inicial y su estructura

`action :` sera el cual nos dara los valores que corresponde a la estructura el cual sabra a cual corresponde por un `Type`

`type` : este es el type, se puede entender como una clave necesaria para saber a cual espacio se esta haciendo refencia y así poder guardar los datos en la estructura correspodiente


La estructura del `studentXuaiiReducer` se construye con un `switch` para evaluar e indentificar el type correspodiente a cada caso

</procedure>


<procedure title="" id="action">

Ubicación: `src/redux/actions/index.ts`

Actions

    export const setDataStudents = (payload:any, identifier:number,year:number,filter:any) => ({
        type:SET_DATA_STUDENT_XUAII,
        payload,
        identifier,
        year,
        filter,
    })

`setDataStudents :` es una función la cual tiene como proposito ejecutar el `type` correspodiente a este caso con el `payload` necesario como los es `year`,`identifier` etc.

</procedure>

<procedure title="" id="use">

Ubicación: `src/store/reducer.ts`

todos los Reducers los asociamos al hook selector , teniendo en cuenta su action y su estado inicial

    
    const reducer = combineReducers({
        able: ableReducer,
        demo: demoReducer,
        studentXuaiiR:studentXuaiiReducer,
        GroupXuaiiReducer:GroupXuaiiReducer,
        XuaiiCoordiReducer:XuaiiCoordiReducer,
        DataRoles:DataRoles
    });
    
    export const useSelector = createSelectorHook<{
        able: typeof AbleState;
        demo: typeof DemoState;
        studentXuaiiR: typeof studentXuaiiRInitial;
        GroupXuaiiReducer: typeof GroupXuaiiRInitial;
        XuaiiCoordiReducer: typeof XuaiiCoordiInitial;
        DataRoles: typeof DataRolesInitial;
    }>();

Ejemplo: 
El ejemplo de use de estos es de la siguiente manera, se hara basado en la información anteriormente recopilada 

    const date = useSelector((state) => state.studentXuaiiR)
    const dispatch = useDispatch();

`data: ` contiene todos los datos del redux que se ha alamacenado por medio del `dispatch` Y la función `setDataStudents`

    dispatch(setDataStudents(data, id, year, value));

`dispatch:` el haces dispatch a los datos para tenerlos y actualizarlos en el estado

</procedure>

