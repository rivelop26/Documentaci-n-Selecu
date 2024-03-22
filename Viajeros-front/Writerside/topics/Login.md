# Login

Login el cual se encarga de gestionar El sistema de autenticación de viajeros, con la intención de obtener la información necesaria del usuario.

<procedure title="Información del archivo" id="file-info">
    <step>
        <p><code>Nombre del archivo: </code> Signln1.tsx</p>
    </step>
    <step>
        <p><code>Ubicación del archivo: </code>src/views/Authentication/Signln/Signln1.tsx</p>
    </step>
</procedure>


Todas las peticiones y consultas se hacen a partir de la baseURL: 

 <code-block lang="plain text">baseURL: "https://api.selecu.net/api-selecu/",</code-block>

## Componente

A continuación, describiremos algunos detalles para la documentación de este código:

	const Login = (props: any) => {

Login = Este es el componente general contiene el código del login 

### Documentación de métodos y parámetros:

	const login = () => {}

LOGIN = Es una función la cual es la encargada de Ejecutar el proceso de autenticación, esta función tiene 2 métodos CRUD


POST el cual es el encargado de hacer la autenticación del usuario y hacer la validación correspondiente.


	const payload = {
   	 username,
   	 password,
    }	;

PAYLOAD = Estos son los datos necesarios que requiere el endPoint para poder procesar la autenticación 

<procedure title="" id="select">

Ruta: BaseURL  Resultado

    {
        token: "",
        roles: "",
        id: 32389,
        username: "",
        fullname: "",
        isPremium: true,
        message: "Autenticación correcta"
    }
Este es el JSON que me deja como respuesta el POST

</procedure>

<procedure title="" id="Select">


GET = Después de haber verificado la validez de los datos procede a solicitar los datos correspondientes al usuario, esto con el fin de tener todas las características de este usuario como: ROL, INSTITUCIÓN, NOMBRE, ETC

#### Ruta: BaseURL  Resultado
    
    {
        id: 329,
        username: 100056,
        fullName: "Santiago",
        email: "correo.com",
        isOnline: false,
        lastLogin: "2024-03-06T00:00:00.000Z",
        numberLoggedIn: 15,
        isInactive: false,
        isPremium: true,
        gender: null,
        roles: "Development",
        created_at: "2024-02-21T13:58:39.254Z",
        updated_at:" 2024-03-06T21:31:56.000Z",
        identification: 1211,
        products: [
            {
            id: 19,
            name: "Expedicion Cerebro",
            created_at: "2024-01-10T17:22:41.447Z",
            updated_at: "2024-02-01T14:15:47.998Z"
            }
        ],
        institution: {
            id: 1,
            name: "Development",
            created_at: "2023-01-30T15:05:47.997Z",
            updated_at: "2023-01-30T15:05:47.997Z"
        }
    }

Este es el JSON que me deja como respuesta el GET
Estos datos son almacenados en El LOCALHOST con la intención de reutilizarlo a lo largo del todo el sistema

Estos datos son necesarios para poder hacer consultas y conocer mucho más que puede o no puede hacer dicho usuario 

</procedure>


	const redirect = (role: string) => {

REDIRECT = Es una función la cual se encarga de hacer la redirección inicial del usuario dónde la web Viajeros.


    useEffect(() => {
        const token = getToken();
            if (token) {
                const user = getUser();
            if (user) {
                redirect(user.roles);
            }
            }
    }, []);


useEffect = Es el efecto inicial que se encarga de validar si ya existe un token para hacer una redirección del usuario y no tenga que iniciar de nuevo  


    const rolesAvailable = ["Admin", "Mentor", "Directive","Coordinador"];

RolesAvailable = Te dice que roles pueden iniciar








