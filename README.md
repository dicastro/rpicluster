Playbook de Ansible para instalación de LXD
  - inicializa LXD en los hosts donde vaya a utilizarse
  - configura el perfil `default`
    - establede el tipo de fs
  - crea 2 perfiles `apps-large` y `apps-small`
    - limitan cores, ram y disco
  - crea perfil `macvlan`

Playbook de Ansible para instalación de Apps
  - Instalacion de postgresql
    - En una máquina que tenga LXD
    - Crea perfil `postgresql`
      - Establede configuración de cloud-init
        - IP fija
        - Hostname
    - Crea contenedor LXD con perfiles `default`, `macvlan`, `apps-large`, `postgresql`
    - En el contenedor instala y configura postgres
  - Instalación de rabbitmq
    - En una máquina que tenga LXD
    - Crea perfil `rabbitmq`
      - Establede configuración de cloud-init
        - IP fija
        - Hostname
    - Crea contenedor LXD con perfiles `default`, `macvlan`, `apps-large`, `postgresql`
    - En el contenedor instala y configura rabbitmq

Playbook de Ansible para instalación de NetData
  - instalar NetData en contenedor LXD
    - actuará como almacén central y para exponer el dashboard
    - https://learn.netdata.cloud/docs/agent/registry/#run-your-own-registry
    - https://learn.netdata.cloud/docs/agent/streaming
  - instalar NetData en el resto de piezas
    - contenedor de postgres
    - contenedor de rabbitmq
    - contenedor de NFS
    - cluster de kubernetes