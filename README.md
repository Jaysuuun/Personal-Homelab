# Personal-Homelab

This is my personal homelab created for educational purposes. This is where I test tools, simulate scenarios, and document them. This is version 1.0 of the homelab. There have been many versions before it, but they were not documented properly; that is why this repo was created.  

# Current Homelab Architecture

  ```mermaid
    architecture-beta
    group wan(internet)[WAN]
    service internet(internet)[Internet] in wan
    service isp(cloud)[ISP] in wan
    service cgnat(cloud)[CGNAT] in wan

    group home(cloud)[Home_Network]
    service router(server)[Home_Router] in home
    service laptop(server)[Laptop_Wazuh_Agent] in home

    group pc(server)[My_PC_Wazuh_Server] in home
    service manager(server)[Wazuh_Manager] in pc
    service filebeat(disk)[Filebeat] in pc
    service indexer(database)[Wazuh_Indexer] in pc
    service dashboard(server)[Wazuh_Dashboard] in pc
    service pcwg(cloud)[WireGuard_PC] in pc

    group remote(cloud)[Remote_VPS]
    service vps(server)[VPS_Wazuh_Agent] in remote
    service vpswg(cloud)[WireGuard_VPS] in remote

    internet:R -- L:isp
    isp:R -- L:cgnat
    cgnat:B -- T:router

    router:R -- L:pcwg
    router:B -- T:laptop
    laptop:R -- L:manager

    internet:B -- T:vps
    vps:B -- T:vpswg
    vpswg:L -- R:pcwg
    pcwg:B -- T:manager

    manager:R -- L:filebeat
    filebeat:R -- L:indexer
    dashboard:B -- T:indexer
  ```
