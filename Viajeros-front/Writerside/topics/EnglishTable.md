# EnglishTable

Información del Archivo:
Nombre:` EnglishTable.tsx`
Ubicación: `src/views/xuaii/Components/EnglishTable.tsx`

se encarga de recibir los datos y gestionarlos para la estructura de la tabla `table` , `tr`, `td , th`

    const EnglishTable = ({englishDate}:typeEnglish) => {

`EnglishTable :` Los datos que son provenientes del board de mentores


### Script {collapsible="true"}

     <Card className="shadow-none">
            <Card.Header className="text-center"><h5>Worldview</h5></Card.Header>
            <Card.Body>
                <table className="table Questions-Table">
                    <thead>
                    <tr>
                        <th>Competencia</th>
                        <th>Nivel</th>
                    </tr>
                    </thead>
                    <tbody>
                    {
                        datosArray.map((item,index)=>{
                            if (item[0] === 'state') {
                                return (
                                    <tr key={index}>
                                        <td><b>Nivel de desempeño</b></td>
                                        <td>{item[1]}</td>
                                    </tr>
                                )
                            } else
                                return (
                                    <tr key={index}>
                                        <td><b>{item[0].charAt(0).toUpperCase() + item[0].slice(1)}</b></td>
                                    <td>{item[1]}</td>
                                </tr>
                            )
                        })
                    }
                    </tbody>
                </table>
            </Card.Body>
        </Card>



#### Tabla {collapsible="true"}
Resultado

![Table_inglés_individual.png](Table_inglés_individual.png)

