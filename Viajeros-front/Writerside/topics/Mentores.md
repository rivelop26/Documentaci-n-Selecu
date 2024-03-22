# Mentores


Información del Archivo:
Nombre:` TribeXuaii.tsx`
Ubicación: `src/views/xuaii/TribeXuaii.tsx`


Información del Archivo:
Nombre:` NomandXuaii.tsx`
Ubicación: `src/views/xuaii/NomandXuaii.tsx`

 <code-block lang="plain text">baseURL: https://api.selecu.net/api-selecu/,</code-block>

Mentores es el Rol de nivel mas bajo el cual cuenta con una sección de Informes con dos sebsecciones

`Grupal` e `Individual`

Estas dos secciones cuentan con información detalla de un grupo por default asignado al mentor el cual con anterioridad inicio sección

## Individual

### Componentes


`Board.tsx`

Ubicación: `src/views/xuaii/Componets/NomandXuaii.tsx`

En este componente se basa toda la logica de datos De los estudiantes idividuales

En este componente hay un useEffect que Obedece a cambios de estados `[year, value, buttonBool1]`

<code-block lang="plain text">
useEffect(() => {
    ...Resto de codigo
}, [year, value, buttonBool1])
</code-block>
## `EndPoints Usados`

`Parametros`

    const params = {
        userId: id,
        type: value,
        year: year
    }

### Area

Las respuestas son Dinamicas y se basan en Los parametros ya establecidos `El año y El Type : entrada o salida`

![Endpoint_Boar_xuaii_1.png](Endpoint_Boar_xuaii_1.png){ border-effect="line" thumbnail="true" width="321"}

#### Response {collapsible="true"}


    [
            {
                "topic": "Matematicas_5",
                "correct_performance": 30,
                "incorrect_performance": 70
            },
            {
                "topic": "Lenguaje_5",
                "correct_performance": 40,
                "incorrect_performance": 60
            },
            {
                "topic": "Competencias Ciudadanas_5",
                "correct_performance": 40,
                "incorrect_performance": 60
            },
            {
                "topic": "Ciencias Naturales_5",
                "correct_performance": 70,
                "incorrect_performance": 30
            }
        ]

#### Uso de datos / Procesos

`For :`
Se ordena los datos y las categorias dentro del arreglo `Data``categorie`
para porder insertarlo en la estructura `ApexAxisChartSeries` y actualizar el estado `setSeries`

`setArea : `
Estado de Las categorias (Matemáticas, Español, etc)

##### Script {collapsible="true"}
    useEffect(() => {
        const Data = [];
        const categorie: any = [];
    
        for (const item of data) {
            Data.push(Math.round(item.correct_performance));
            const areaName = item.topic.split('_')
            if (areaName[0] === 'Matematicas') categorie.push('Matemáticas');
            else categorie.push(areaName[0].split(' '));
        }
        const dataSeries: ApexAxisChartSeries = [
            {name: "%", data: Data}
        ]
    
        setArea(() => ({
            options: {
                xaxis: {
                    categories: categorie,
                    title: {
                        text: 'Área',
                        style: {
                            fontWeight: 200,
                            fontSize: '15px',

                        }
                    }
                }
            }
        }))
        setSeries(dataSeries)
    
        }, [data])

#### Tablas


![EndPoint_Xuaii_2_tablas.png](EndPoint_Xuaii_2_tablas.png){ border-effect="line" thumbnail="true" width="321"}

#### Response tabla : {collapsible="true"}
Esta es la estructura del JSON que tiene como respuesta, Es simplemente una estructura por lo cual pueden haber dentro del `JSON` muchos objetos

    [
        {
            "ID_Usuario": "78",
            "Nombre": "Alvaro Enrique Montoya Hernandez",
            "Pregunta": "¿Cuántas piedras recolectó Xuaii al principio del camino?",
            "Competencia": "Resolución de Problemas",
            "Area": "Matematicas_3",
            "Resultado": "Incorrecta"
        }
    ]

#### Proceso tablas competencias
`agrupadoPorArea :` Separación de los datos del `array` ordenandolos por area y actualizando el `setQuestionsSplit`

##### ScriptTabla {collapsible="true"}
    useEffect(() => {
        let agrupadoPorArea = questions.reduce((acumulador: any, objeto: any) => {
            let are = objeto.Area.split('_')[0];
            if (!acumulador[are]) {
                acumulador[are] = [];
            }

            acumulador[are].push(objeto);
            return acumulador;
        }, {});
        let resultArray: [][] = Object.values(agrupadoPorArea);
        setQuestionsSplit(resultArray);
    }, [questions])

usado en el mapeo de tablas

![MapDePreguntasTabla.png](MapDePreguntasTabla.png){ border-effect="line" thumbnail="true" width="321"}


### Competencia
`EndPoint`

![EndPoint_xuaii_3_compentencia.png](EndPoint_xuaii_3_compentencia.png){ border-effect="line" thumbnail="true" width="321"}

#### Response Compentencia : {collapsible="true"}

Esta es la estructura del JSON que tiene como respuesta, Es simplemente una estructura por lo cual pueden haber dentro del `JSON` muchos objetos

    [
    {
        "Resolución de Problemas": {
            "5": {
            "area": "Matematicas",
            "correct_performance": 0,
            "incorrect_performance": 100
            }
        }
    },
    {
        "Razonamiento": {
            "5": {
            "area": "Matematicas",
            "correct_performance": 60,
            "incorrect_performance": 40
            }
        }
    }
    ]

#### Proceso
`For:` Se separan las competencias por `areas` para insertalos en la estructura `ApexAxisChartSeries` y actualización del estado

#### Fragmento {collapsible="true"}

    for (const item of dataCompentecy) {
        for (const itemKey in item) {
            const value = item[itemKey]
            for (const valueKey in value) {
                switch (value[valueKey].area) {
                case 'Matematicas':
                    let a = value[valueKey].correct_performance
                    Matematicas.push(Math.round(a))
                    MatematicasCompetency.push(itemKey.split(' '))
                    break;
                case 'Lenguaje':
                    let e = value[valueKey].correct_performance
                    Lenguaje.push(Math.round(e))
                    LenguajeCompetency.push(itemKey.split(' '))
                    break;
                case 'Ciencias Naturales':
                    let i = value[valueKey].correct_performance
                    CienciasNaturales.push(Math.round(i))
                    CienciasNaturalesCompetency.push(itemKey.split(' '))
                    break;
                default:
                    let o = value[valueKey].correct_performance
                    are4.push(Math.round(o))
                    are4OCompetency.push(itemKey.split(' '))
                }
            }
        }
    }



### Worldview_
![EndPoint_Xuaii_5_inglés.png](EndPoint_Xuaii_5_inglés.png){ border-effect="line" thumbnail="true" width="321"}

#### Response Worldview {collapsible="true"}

Este es el resultado unico que tiene este endPoint ya que trae el desempeño exacto del estudiante actual

    {
        "Listening_and_Vocabulary": "A2",
        "reading": "A2",
        "general": "A2",
        "state": "Logrado"
    }

#### Proceso Wordlview

Graficación de `datos` en tablas sencillas

      <EnglishTable
         englishDate={english}
      />

## Grupal


#### Area grupal


useEffect inicial

    useEffect(() => {

        const user: UserInterface = getUser();
        if (user.roles !== 'Coordinador') setUser(user);
    }, []);

Como ya sabemos este componente `TribeXuaii.tsx` es uno de los componentes usados en todos los los roles para la presentación de datos a los usuarios, todo esto con la inteción de reducir la cantidad de codigo duplicado y reutilizar lo que ya esta construido, pero con unas ligeras variaciones dependiendo del rol 

`useEffect:` Este componente tiene este efecto inicial con la inteción de verificar si estamos en un mentor o coordinador , solo me permitira actualizar el usuario si es `mentor`

ya que cuando somos coodinadores o Directivos usamos otro metodo para obtener el usuario y los datos pertinentes dependiendo de los valores del los filtro, como `grupo` etc
<procedure title="" id="area">

Endpoint area

    .patch<[AreaDataXuaiiGrupal]>('responses-xuaii/mentor/topic/performance/',params)
    .then((response)=>{
         const data = response.data
         setData(data);


<procedure title="Response" id="area-">

    [
        {
            "topic": "Lenguaje_3",
            "correctPerformance": 47.58620689655172,
            "incorrectPerformance": 52.41379310344828,
            "count": 29
        },
        {
            "topic": "Matematicas_3",
            "correctPerformance": 32.758620689655174,
            "incorrectPerformance": 67.24137931034483,
            "count": 29
        }
    ]

Type

    export interface AreaDataXuaiiGrupal{
        topic:string,
        correctPerformance:number,
        incorrectPerformance:number
    }

</procedure>

    useEffect(()=>{

        const Data = [];
        const categorie:any = [];

        for (const item of data) {
            Data.push(Math.round(item.correctPerformance));
            const areaName = item.topic.split('_')
            if(areaName[0] === 'Matematicas') categorie.push('Matemáticas');
            else  categorie.push(areaName[0].split(' '));
        }

`for:` Al igual que en la sección de mentores individual, se separa los valores de los datos en el arreglo `Data` para forma la estructura de números del `ApexAxisChartSeries`

`categorie:` Del mismo modo separamos las area en categorías en el orden correcto para que coincidan con sus valores

Estrutura dela libreria ApexAxisChartSeries

        const dataSeries:ApexAxisChartSeries =[
            {name:"%",data: Data}
        ]

Estado de las categorias

        setArea(()=>({
            options: {
                xaxis: {
                    categories: categorie,
                    title:{
                        text: 'Área',
                        style:{
                            fontWeight: 200,
                            fontSize: '15px',

                        }
                    }
                }
            }
        }))

Estado de las series

        setSeries(dataSeries)

    },[data])
</procedure>


#### Competencia grupal

<procedure title="" id="Competencia">

Endpoint competencia

    apiEva
    .patch<[CompetencyDataXuaiiGrupal]>('responses-xuaii/mentor/competency/performance/',params)
    .then((response)=>{
        const data = response.data
    setDataCompetency(data);

<procedure title="Response" id="Competencia-">

    [
        {
            "competency": "Comprensión Inferencial",
            "grado": "3",
            "area": "Lenguaje",
            "correct_performance": 49.42528735632184,
            "incorrect_performance": 50.57471264367817,
            "count": 29
        },
        {
            "competency": "Comprensión Crítica",
            "grado": "3",
            "area": "Lenguaje",
            "correct_performance": 52.87356321839081,
            "incorrect_performance": 47.12643678160919,
            "count": 29
        },

    ]

Type

    export interface CompetencyDataXuaiiGrupal{
        "competency": string,
        "grade": string,
        "area": string,
        "correct_performance": number,
        "incorrect_performance": number,
        "count": number
    }

</procedure>


    useEffect(()=>{
        const processData = (area:string, data:React.SetStateAction<any>, competency:string, setState:React.SetStateAction<any>, optionsState:React.SetStateAction<any>) => {
            const values = [];
            const competencyValues = [];

            for (const item of data) {
                if (item.area === area) {
                    values.push(Math.round(item.correct_performance));
                    competencyValues.push(item.competency.split(' '));
                }
            }

`for:` Separamos los valores de la competencia , los valores para las `series ` y las `competencias` para las categorias

            if (values.length < 2 && values.length > 0) {
                values.push(0);
            }
            
            const series = [{ name: area, data: values }];

`series:` estructura del ApexAxisChartSeries

            setState(series);

`setState:` estado que le pasamos

            optionsState({
                options: {
                    xaxis: {
                        categories: competencyValues,
                        title: {
                            text: 'Competencia',
                            style:{
                                fontWeight: 200,
                                fontSize: '15px',

                            }
                        },
                    },
                },
            });
        };

`optionsState:` optionsState estado de las categorias


`areas:` areas

    const areas = ['Matematicas', 'Lenguaje', 'Ciencias Naturales', 'Competencias Ciudadanas'];


`stateVariables:` los estados que se actualizan en orden deacuerdo a las areas

       const stateVariables = [
            [SetMatematicas, setOptionsMatematica],
            [Setlenguaje, setOptionslenguaje],
            [SetNaturales, setOptionsNaturales],
            [SetAre4, setOptionsAre4],
        ];

`areas:` recorremos todos los datos por area y competencia para ordenarlos en arreglo

        areas.forEach((area, index) => {
            const [setState, optionsState] = stateVariables[index];
            processData(area, dataCompentecy, "", setState, optionsState);
        });

    },[dataCompentecy])

</procedure>

#### Wordlview

<procedure title="" id="Wordlview">
Endpoint Wordlview

    apiEva
    .patch<worldviewXuaii>('responses-xuaii/mentor/worldview/performance/',params)
    .then((response)=>{
        const data = response.data
        setWorldview(data);

<procedure title="Response" id="Wordlview-">

    {
        "reading": {
            "PRE A1": 3,
            "A1": 11,
            "A2": 15,
            "B1": 0,
            "B2": 0
        },
        "listening_and_Vocabulary": {
            "PRE A1": 4,
            "A1": 22,
            "A2": 3,
            "B1": 0,
            "B2": 0
        },
        "general": {
            "PRE A1": 9,
            "A1": 6,
            "A2": 14,
            "B1": 0,
            "B2": 0
        }
    }

Type

    export interface worldviewXuaii{
    [key: string]:{
        [key: string]: number,
    }

}

</procedure>

En general en todos los procesos que se hacen con los datos para la graficación de las charts , se enfoca hacia la misma guia o estructura, todos los datos se procesan u ordenan separandolos de modo que los `values` queden en un arreglo en orden deacuerdo a las categorias o nombres

    useEffect(()=>{
`stateVariables:` el listado de los estados y variables por nombre del item a calificar `reading etc`

        const stateVariables = {
           "listening_and_Vocabulary": setListening,
            "general":setGeneral,   
            "reading":setReading,
        };

`for:` se ordena los values en el arreglo series para insertarlos en los estados correspodiente de `stateVariables`, se repite los procesos anteriores

       for (const key in worldview) {
           let value = worldview[key]
           let series = [];
           let state = stateVariables[key as keyof typeof stateVariables];

           for (const valueKey   in value) {
               series.push(value[valueKey])
           }
           state([{name:key,data:series}])
       }

    },[worldview])


</procedure>

#### filtros

para el filtrado de datos se usa estos 2 filtros, `FilterProgress` esta primera sección es para filtrar datos que normalmente se usa en todos los lugares en los cuales se rederizan datos 

`OptionsFilter:` Es una sección sencilla que se usda para cambiar entres las diferentes tablas , pero como se ve se le pasan unos valores como `year` y `value` que son dinamicas para descargar un reporte en excel
   
     return (
        <>
             <FilterProgress
                setYear={setYear}
                setValue={setValue}
                groupSede={groupSede}
                setUpdateGroup={setUpdateGroup}
                institutions={institutions}
                setCampus={setCampus}
            />

            <div className="BodyXuaiiTribe" id="">
            <OptionsFilter
                style="Left-Nav"
                setButtonBool1={setButtonBool1}
                english={true}
                year ={year}
                value ={value}
            />
        />

