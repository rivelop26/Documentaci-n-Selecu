# Product



Información del Archivo: 
Nombre:` index.tsx`
Ubicación: `src/App/layaout/AdminLayout/Navigation/NavContent/NavGroup/index.tsx`

 <code-block lang="plain text">baseURL: https://api.selecu.net/api-selecu/,</code-block>

## Componente

Componente NavGroup 

<code-block lang="plain text">const navGroup = (props: NavGroupProps) => {</code-block>



### Funcionalidad
Este componente es el encargado de extraer y ordenar los productos del usuario después de registrar


#### End point

<code-block lang="xml">
 apiAuth.get("user/products/" + id).then((response) => {
            setDataProducts(response.data)
        });
</code-block>

Resultado: 

Esta es la estructura de los productos y puede cambiar dependiente del `rol` o `usuario`

    [
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
        }
    ]


#### Proceso {collapsible="true"}
 
`finalArray:` arreglo que simplifica el producto a xuaii, con la inteción de tener xuaii como el producto principal en todos los roles

`if:` inicialización del producto, el cual se guarda en las cookies de la web para hacer uso de él.

`useEffect` efecto que reacciona al cambio del estado de los productos con respecto al usuario

     React.useEffect(() => {
            const val = document.cookie.split(";")

            const arrayWithXuaii = dataProducts.filter(item => item.name === "Buscando a Xuaii");
            const arrayWithoutXuaii = dataProducts.filter(item => item.name !== "Buscando a Xuaii");
            const finalArray = arrayWithXuaii.concat(arrayWithoutXuaii);

            if (finalArray.length > 0){
                if (document.cookie === "" || val[0] === "t=null") {
                    document.cookie = "t=" + finalArray[0].name;
                    setProducts(finalArray);
                }
                else setProducts(finalArray);
            }

    }, [dataProducts]);


#### GET cookies

<procedure title="Inject a procedure" id="inject-a-procedure">
     <code-block>
             function getT() {
                    const cookies = document.cookie.split(";");
                        for (let i = 0; i < cookies.length; i++) {
                        const cookie = cookies[i].trim();
                        if (cookie.startsWith(`t=`)) {
                        return cookie.substring("t".length + 1, cookie.length);
                        }
                    }
                }
        </code-block>

`const cookies:` Quitamos todos los espacios del producto que tenemos en las cookies

`if:` Verificamos si la cookie inicia con `t` ya que con esto identificamos si es o no un producto,

`return:` regresamos el valor sin el `t=` para tener el producto tal como es: `T2,T3,Xuaii`etc.

Existe una función llamada `cookies` que hace esto por ti sin tener que duplicar el codigo
`ubicación:` src/views/Pihe/cookies.ts

</procedure>

#### ITEM | COLLAPSE

<procedure title="" id="ITEM | COLLAPSE">

  ![itemCollapse.png](itemCollapse.png){ border-effect="line" thumbnail="true" width="321"}

     if (getT() === "EExperiencias Comfama" && parseInt(key) > 0) {
                return
            }  else {
                switch (item.type) {
                    case "collapse":
                        return <NavCollapse key={item.id} collapse={item} type="main"/>;
                    case "item":
                        return <NavItem key={item.id} item={item}/>;
                    default:
                        return false;
                }
            }

En esta sección todo va enfocado en ordenar las sseciones del menu lateral dependiendo de la estructura anteriormente definida

`if:` Si el producto que se seleciona es EExperiencias Comfama no se continúa definiendo la estructura

`else:` dependiendo el caso de la estructura definimos si es un `collapse (se puede esplegar y tiene dentro item del menu)`, o `item (es un objeto con un link para redireccionar)` 


Ubicación estructura: ` src/menu-items.ts`

</procedure>





<procedure title="" id="Select">

SELECT product 
<procedure title="" id="Select">

![select_Product_menu.png](select_Product_menu.png){ border-effect="line" thumbnail="true" width="321"}

</procedure>
`products:` Arreglo de productos mapeado en opciones de un select de productos
`res:` item del arreglo el cual se compara con el de las cookies para saber si está o no selecionado

<procedure title="" id="Select">

    getT() === 'PIHE' || 'Directivo' || 'EExperiencias Comfama' ? (
    rs.name.substring(0)
    ) :
    "Temporada " + rs.name.substring(1)


Se usa para identificar si es una temporada de seluverso o un prodcuto diferente para agregar el inicio `TEMPORADA + T2` más la temporada
</procedure>
</procedure>