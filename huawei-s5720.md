# Configuração Switch HUAWEI S5720

1. Conecte o cabo serial a porta console do switch e entre com o usuário e senha

    ```bash
    admin@huawei.com
    Admin@huawei.com
    ```

2. Configuração de interface VLAN

    ``` bash
    <HUAWEI> system-view
    [HUAWEI] sysname HUAWEI-SW01
    [HUAWEI-SW01] interface MEth 0/0/1
    [HUAWEI-SW01] ip address 192.168.0.1 24
    [HUAWEI-SW01] quit
    [HUAWEI-SW01] vlan 2000
    [HUAWEI-SW01-vlan2000] name VLAN_MANAGEMENT
    [HUAWEI-SW01-vlan2000] quit
    [HUAWEI-SW01] interface Vlanif 2000
    [HUAWEI-SW01-Vlanif2000] ip address 10.1.0.xxx 255.255.xxx.xxx
    [HUAWEI-SW01-Vlanif2000] quit
    ```

3. Criar VLAN

    ``` bash
    [HUAWEI-SW01] vlan 10
    [HUAWEI-SW01-vlan10] name VLAN_WIFI
    [HUAWEI-SW01-vlan10] description VLAN_WIFI
    [HUAWEI-SW01-vlan10] quit
    [HUAWEI-SW01] display vlan
    ```

4. Configurar VLAN em porta de acesso

    ``` bash
    [HUAWEI-SW01] interface gigabitethernet 0/0/1
    [HUAWEI-SW01-GigabitEthernet0/0/1] port link-type access
    [HUAWEI-SW01-GigabitEthernet0/0/1] port default vlan 10
    [HUAWEI-SW01-GigabitEthernet0/0/1] quit
    ```

5. Configurar porta Trunk

    ``` bash
    [HUAWEI-SW01] clear configuration interface gigabitethernet 0/0/48
    [HUAWEI-SW01] interface gigabitethernet 0/0/48
    [HUAWEI-SW01-GigabitEthernet0/0/48] restart    
    [HUAWEI-SW01-GigabitEthernet0/0/48] port link-type trunk
    [HUAWEI-SW01-GigabitEthernet0/0/49] port trunk allow-pass vlan 10 2000 (Para todas as VLANs "port trunk allow-pass vlan all")
    [HUAWEI-SW01-GigabitEthernet0/0/49] quit
    ```

6. Configurar rota padrão
   
    ``` bash
    [HUAWEI-SW01] ip route-static 0.0.0.0 0.0.0.0 10.1.0.xxx
    [HUAWEI-SW01] quit
    ```

7. Salve as Configurações
   
    ``` bash
    [HUAWEI-SW01] display current-configuration
    [HUAWEI-SW01] quit
    <HUAWEI-SW01> save
    ```


8. Configurar Senha para acesso WEB
   
    ``` bash
    <HUAWEI-SW01> system-view
    [HUAWEI-SW01] aaa
    [HUAWEI-SW01-aaa] local-user huawei password irreversible-cipher huawei@123
    [HUAWEI-SW01-aaa] local-user huawei service-type http
    [HUAWEI-SW01-aaa] local-user huawei privilege level 3
    [HUAWEI-SW01-aaa] quit
    [HUAWEI-SW01] quit
    <HUAWEI-SW01> save
    <HUAWEI-SW01> reboot
    ```

9. Configurar Stack
   
    ``` bash
    <HUAWEI-SW01> system-view
    [HUAWEI-SW01] interface stack-port 0/1
    [HUAWEI-SW01-port0/1] port interface XGigabitEthernet1/0/1 enable
    [HUAWEI-SW01-port0/1] quit
    [HUAWEI-SW01] interface stack-port 0/2
    [HUAWEI-SW01-port0/2] port interface XGigabitEthernet1/0/2 enable
    [HUAWEI-SW01-port0/2] quit
    [HUAWEI-SW01] stack slot 0 priority 200
    [HUAWEI-SW01] display stack configuration
    [HUAWEI-SW01] quit
    ```

10. Remover configuração Stack
   
    ``` bash
    <HUAWEI-SW01> system-view
    [HUAWEI-SW01] interface stack-port 0/1
    [HUAWEI-SW01-port0/1] shutdown port interface XGigabitEthernet1/0/1
    [HUAWEI-SW01-port0/1] undo port interface XGigabitEthernet1/0/1
    [HUAWEI-SW01-port0/1] quit
    [HUAWEI-SW01] quit
    ```

11. Resetar Configuração

    11.1. Opção 1

        ``` bash
        <HUAWEI-SW01> reset saved-configuration
        <HUAWEI-SW01> reboot fast
        ```
    11.2. Opção 2

        Durante a inicialização pressione Ctrl + B e digite a senha do menu BootROM avançada da seguinte forma:

        A senha padrão é Admin@huawei.com

        BootLoad Menu

            1. Boot with default mode
            2. Enter serial submenu
            3. Enter startup submenu
            4. Enter ethernet submenu
            5. Enter filesystem submenu
            6. Enter password submenu
            7. Clear password for console user
            8. Reboot
            (Press Ctrl+E to enter diag menu)

            Enter your choice(1-8): 

        selecione 7 para limpar a senha do usuário do console e depois selecione 1.
        
    #### Obs.: não salve a configuração antes do reboot