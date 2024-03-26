# Coordinador


Información del Archivo:
Nombre:` CoordinadorXuaiiG.tsx`
Ubicación: `src/views/xuaii/Roles/CoordinadorXuaiiG.tsx`


Todos los `roles` de los productos de viajeros van enfocados en focados en los `mismos componentes`, esto quiere decir que no fue necesario clonar codigo de los componentes ya que fue util reutilizar el mismo de `Mentores`

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



    useEffect(() => {
      if(Redux.degree?.id)  getMentors(Redux.degree?.id);
    }, []);

`useEffect:`Este es el efecto inicial con el cuál podemos indentificar si fue la primera carga del componente o fue un cambio de componente como tal
cuando tenemos datos del grupo quiere decir que ya estabamos navegando e interactuando con los grupos en difentes componentes por lo cual podemos continuar usando el actual sin necesidad de cambiarlo por uno incial

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























