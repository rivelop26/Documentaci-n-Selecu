# Pihe

Información del Archivo:
Nombre:` tribe.tsx`
Ubicación: `src/views/pihe/tribe.tsx`

En pihe todo se maneja de la misma forma que en el resto de componente los cuales usan graficas, solo que los datos son transferidos de una forma particular

`useUpdateDate:` todos los datos necesarios se traen del hooks `useUpdateDate`.

`data`: 

`outputData:` son los datos de salida

    const {
        data,
        outputData
    }:Counter = useUpdateDate();



<procedure title="" id="hooks-">

    const useUpdateDate = ()=>{

    const [data,setData] = useState<ApexAxisChartSeries>([]);
    const [outputData,setOutputData] = useState<ApexAxisChartSeries>([]);

    const [user,SetUser] = useState<UserInterface>();

    const [entry,SetEntry] = useState<any | null>({
        Medal1:{},
        Medal2:{},
        Medal3:{},
    });

    const [output,SetOutput] = useState<any | null>({
        identificationMedals:{
            Medal1:{},
            Medal2:{},
            Medal3:{},
        },
        comparationMedals: {
            Medal1:{},
            Medal2:{},
            Medal3:{},
        }

    });
    const location: any = useLocation();
    const props: any = location.state;

    useEffect(()=>{
        const User: UserInterface = getUser();
        SetUser(User);

    },[])

    const GetEntry=()=>{
       /*
        BaseApi.get(`entry/groupStats/${user?.id}`).then((response)=>{
            const data = response.data
            SetEntry(data);
        })

        */
    }
    const GetOutPut=()=>{
        BaseApi.get(`output/groupStats/${user?.id}`).then((response)=>{
            const data = response.data
            SetOutput(data);
        })
    }

    useEffect(()=>{
       if (user?.id){
            GetEntry();
            GetOutPut();
       }
    },[user])

    useEffect(()=>{

      const outputData1: number[] = (Object.values(output.identificationMedals.Medal1));
      const outputData2: number[] = (Object.values(output.identificationMedals.Medal2));
      const outputData3: number[] = (Object.values(output.identificationMedals.Medal3));

      const outputDataComparation1: number[] = (Object.values(output.comparationMedals.Medal1));
      const outputDataComparation2: number[] = (Object.values(output.comparationMedals.Medal2));
      const outputDataComparation3: number[] = (Object.values(output.comparationMedals.Medal3));

        const outPutDataChartIdentification: ApexAxisChartSeries = [
            { name: 'Bajo', data: outputData1 },
            { name: 'Medio', data: outputData2 },
            { name: 'Avanzado', data: outputData3 },
        ];

        const outPutDataChartComparation: ApexAxisChartSeries = [
            { name: 'Bajo', data: outputDataComparation1 },
            { name: 'Medio', data: outputDataComparation2 },
            { name: 'Avanzado', data: outputDataComparation3 },
        ];

        setData(outPutDataChartComparation);
        setOutputData(outPutDataChartIdentification);

    },[output])


    return{
        data,
        outputData
    }

</procedure>






























