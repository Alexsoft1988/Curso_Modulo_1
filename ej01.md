# JOBS

1. Crear 01-mi-primer-job
    1. Crear proyecto del estilo libre.
        * Nombre: 01-mi-primer-job
        * Description: 01-mi-primer-job
        * Build --> Execute shell --> echo "hola mundo"

1. Crear Folder de pipelines-estilo-libre y mover job 01-mi-primer-job
    1. Crear Folder
        * Nombre: pipelines-estilo-libre
        * Description: pipelines-estilo-libre
    2. Mover job 01-mi-primer-job a folder pipelines-estilo-libre

1. Editar 01-mi-primer-job
    1. Edita proyecto del estilo libre.
        * Nombre: 01-mi-primer-job
        * Description: Primer job
        * Build --> Execute shell --> echo "La fecha del sistema es $(date)"

1. Crear job2
    1. Crear proyecto del estilo libre.
        * Nombre: 02-job
        * Description: Segundo job
        * Build --> Execute shell --> 
        ```console
        NOMBRE="AUTO"
        echo "La fecha del sistema es $(date) - $NOMBRE" >> /tmp/demo.txt
        ```        
        * Verificar
        ```console
        docker exec -it jenkins bash
        ls -lt /tmp
        cat /tmp/demo.txt
        ``` 
        
1. Crear job3

    1. Crear /opt/script.sh
    ```console
    #!/bin/bash

    NOMBRE=$1
    APELLIDO=$2
    MOSTRAR=$3

    if [ "$MOSTRAR" = "true" ]; then
        echo "Hola, $NOMBRE, $APELLIDO"
    else
        echo "Si quieres ver el nombre, selecciona MOSTRAR"
    fi
    ``` 

    ```console
    docker cp script.sh jenkins:/opt/script.sh
    docker exec -it jenkins bash
    ls -lt /opt/
    chmod +x /opt/script.sh
    ``` 
    
    1. Crear proyecto del estilo libre.
    * Nombre: 03-job
    * Description: Tercer job
    * Build --> Execute shell -->
    ```console
    NOMBRE="DEV"
    APELLIDO="OPS"
    MOSTRAR=true

    /opt/script.sh $NOMBRE $APELLIDO $MOSTRAR
    ``` 

1. Clonar 03-job y renombrarlo a 04-job y agregarle parámtros
    1. En la sección **General**, seleccionar _"This project is parameterized"_
    1. Agregar 3 parámetros:
        | NAME        | TYPE           | Value  |
        | ------------- |:-------------:|:-----|
        | NOMBRE        | String        | DEV |
        | APELLIDO      | String        | OPS |
        | MOSTRAR       | Boolean       | true |
        
1. Crear vistas y asignarles jobs diferentes
