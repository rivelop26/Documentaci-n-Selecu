# Coordinador


Información del Archivo:
Nombre:` CoordinadorXuaiiG.tsx`
Ubicación: `src/views/xuaii/Roles/CoordinadorXuaiiG.tsx`


Todos los `roles` de los productos de viajeros van enfocados en focados en los `mismos componentes`, esto quiere decir que no fue necesario clonar codigo de los componentes ya que fue util reutilizar el mismo de `Mentores`

Coordinador grupal y Directivo grupal es lo mismo , lo unico que cambia es que el componente directivo le pasa al componente coodinador un estado con todas las sedes

Como se usaba en mentores el componente `TribeXuaii.tsx` des mismo modo le pasamos información desde una capa mas arriba a este componente 

<procedure title="" id="coordinador-">

    const getGroup = (id: number) => {
        if (user?.id) {
            api.get("groups/allByInstitution/" + id, {headers}).then((response) => {
                const data = response.data;
                setGroups(data);
            });
        }
    };

response 

Este endpoint me da todos los grupos de una sede en específico

    [
        {
            "id": 4,
            "name": "Segundo A - Tarde",
            "number": "12200017",
            "grade": 2,
            "created_at": "2023-03-23T19:47:31.014Z",
            "updated_at": "2024-01-25T16:22:55.342Z"
        },
        {
            "id": 5,
            "name": "Segundo B - Mañana",
            "number": "12200016",
            "grade": 2,
            "created_at": "2023-03-23T19:53:21.441Z",
            "updated_at": "2024-01-25T16:22:55.504Z"
        },
        {
            "id": 6,
            "name": "Tercero A - Mañana",
            "number": "12300010",
            "grade": 3,
            "created_at": "2023-03-23T19:54:33.559Z",
            "updated_at": "2024-01-25T15:31:49.918Z"
        },
        {
            "id": 67,
            "name": "Tercero A - Tarde",
            "number": "12300012",
            "grade": 3,
            "created_at": "2023-03-31T19:57:38.043Z",
            "updated_at": "2024-01-25T15:27:41.300Z"
        },
        {
            "id": 73,
            "name": "Quinto A - Mañana",
            "number": "12500007",
            "grade": 5,
            "created_at": "2023-04-11T14:03:42.454Z",
            "updated_at": "2023-10-09T21:20:30.475Z"
        }
    ]

`getGroup:` Esto ya hace parte de Directivo, se usa para obtener los grupos de una sede o Institución

    useEffect(() => {
        if (campus?.id != 0) {
            getGroup(campus?.id);
        } else if (user?.roles == "Directive" && institutions[0].id !== 0) {
            getGroup(institutions[0].id);
        } else if (data.institution.id != 0) {
            getGroup(data.institution.id);
        }
    }, [data, campus?.id, campus, institutions])

Esto es 

`campus :` 

    //GET-MENTORS
    function getMentors(id: number) {
    api
        .get(`/group_mentors_user/mentors/group/${id}`, {headers})
        .then((response) => {
            setMentor(response.data);
        })
        .catch((error) => {
        });
    }
`getMentors:`Obtenemos los mentores que tiene asignado un grupo, la inteción de esto es obtener todos los estudiantes mediante el id del mentor

response 

la respuesta sera la estructura normal de un usuario  

    {
        "id": 00000,
        "username": "45424",
        "fullName": "Directivo",
        "email": "vwqw",
        "isOnline": false,
        "lastLogin": "2024-03-27T00:00:00.000Z",
        "numberLoggedIn": 47,
        "isInactive": false,
        "isPremium": true,
        "gender": "",
        "roles": "Directive",
        "created_at": "2023-04-25T19:41:47.817Z",
        "updated_at": "2024-03-27T18:24:12.000Z",
        "identification": "30461",
        "products": [
            {
            "id": 12,
            "name": "Directivo",
            "created_at": "2023-08-23T21:12:04.799Z",
            "updated_at": "2023-08-24T15:35:28.818Z"
            },
            {
            "id": 15,
            "name": "Buscando a Xuaii",
            "created_at": "2023-11-02T14:14:28.972Z",
            "updated_at": "2023-11-02T14:14:28.972Z"
            },
        ],
        "institution": {
     
        }
    }

`useEffect :` este efecto cambia respecto al cambio de los estados asociados a los grupos, este se da lugar una vez que ya tenemos los grupos asociados a una sede en especial o selecionamos un grupo en particular

`if:` si en la carga inicial no hemos selecionado un grupo, entonces el tomara por default el tercer grupo

`else if:` si ya la carga que tiene el effecto se debe a que cambiamos el estado de los grupos porque selecionamos uno entonces ingresamos unicamente al segundo codicional

`else if:` Este condicional final solo sera ingresado una vez que ya tenemos un grupo en el estado global, y la inteción de esto es poder maner los datos del grupo independientemente de el componente en el que estamos y lo requerimos


    useEffect(() => {
        if (updateGroup?.id === 0 && groups[2]?.id != 0 && !Redux.degree?.id ) {
          getMentors(groups[2]?.id);
        } else if (updateGroup && user?.roles == "Coordinador" || user?.roles == "Directive" && updateGroup?.id !== 0 && !Redux.degree?.id) {
            getMentors(updateGroup?.id);
        }
        else if (Redux.degree?.id){
            getMentors((Redux.degree?.id));
        }
    }, [updateGroup, groups,Redux.degree?.id]);

`useEffect:`Este es el efecto inicial con el cuál podemos indentificar si fue la primera carga del componente o fue un cambio de componente como tal
cuando tenemos datos del grupo quiere decir que ya estabamos navegando e interactuando con los grupos en difentes componentes por lo cual podemos continuar usando el actual sin necesidad de cambiarlo por uno incial


    useEffect(() => {
      if(Redux.degree?.id)  getMentors(Redux.degree?.id);
    }, []);


`TribeXuaii :` Este es el componente `TribeXuaii` el cual es el mismo usado en grupal mentores coodinadores y directivos

    return (
        <div>
            <div className={!individual ? "BodyDirectiveI" : ''}>
                {
                    <>
                        <TribeXuaii
                            idGroup={mentor}
                            groupSede={groups}
                            setUpdateGroup={setUpdateGroup}
                            updateGroup={updateGroup}
                            setCampus={setCampus}
                            setIndividual={setIndividual}
                            individual={individual}
                        />
                    </>
                }
            </div>
        </div>
    );


</procedure>


### Coordinador Individual

El coordinador individual es los mismo que individual de mentores, ya que lo unico que cambia es la forma en la que se recibe los datos.

cuando estamos en coordinador y vamos a individual lo que hacemos es que tomamos la infomación necesaria desde el estado Global Redux así ahorrando el proceso de tener que pasar datos 
entre componentes.

Aquí podemos notar la diferencia entre los 3 roles en el componente `NomandXuaii.tsx`

Ubicación:  `src/views/xuaii/NomandXuaii.tsx`



`REDUX:` Cómo ya se había explicado en el el documento de Redux, podemos encontrar el store DataRoles, de esta forma podemos obtener la información que tenemos almacenada
    
    //REDUX =====================================
    const Redux = useSelector((state) => state.DataRoles)
    //===========================================

`useEffect:` Aquí es donde podemos indentificar la diferencia entre los 3 roles, 

`getUser():` Es una función el cual me trae del localStorage la información del usuario logiado.

`if:` Si El Redux tiene algún dato de grupo eso quiere decir que estamos en `Coordinador` o `Directivo`, de esta forma podemos saber si estamos en otro rol

`else if:` Este codicional es cuando realmente estamos en mentores ya que los datos de los grupo nunca cambiaran ya que requiere de otro rol para hacerlo por eso tomamos los `datos`
del usuario inicial y ya hacemos la operaciones necesarias

    //INITIAL EFFECT =======================
    React.useEffect(() => {
        const user: UserInterface = getUser();
        if (Redux.degree?.id)getMentors(Redux.degree?.id);
        else if (user.roles === "Mentor")setUser(user);
    }, []);
    //================================


`useEffect:` ya en este efecto el cambio se debe a que se actualizo el estado global con `datos nuevos`, esto se usa para `coordinador` y `directivos`

`Redux.degree?.id:` lo que almacenamos todo el tiempo es el `id` del grupo y el `nombre` y ya con obtenemos el mentor

    React.useEffect(()=>{
        if (Redux.degree?.id)getMentors(Redux.degree?.id);
    },[Redux.degree?.id])































