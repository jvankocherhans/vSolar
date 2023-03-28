# vSphere Environment

Damit wir unsere vSolar Applikation während der Entwicklung ausgiebig testen können, haben wir wir bei uns Zuhause eine eigene vSphere Umgebung aufgebaut. Dabei haben wir beachtet, dass wir die Features: Active Directory Einbindung, VM Migration und die Erstellung diverser Netzwerksegmente im Aufbau miteinbeziehen, um eine möglichst Praxisnahe Umgebung zu erhalten.

## Verwendete Systeme

Grunstätzlich verwenden wir jeweils die neuste Version von jedem System. Die Lizenzschlüssel konnten wir entweder durch die Partnerschaften unserer Schule [Itacademy](https://itacademy.brightspace.com/d2l/login?sessionExpired=0&target=%2fd2l%2fhome) und [Azure](https://portal.azure.com) oder im Falle von ESXI, direkt beim Hersteller [VMware Customerconnect](https://customerconnect.vmware.com/evalcenter?p=free-esxi8) erwerben.

|System|Anzahl|Version|
|-|-|-|
|VMware ESXI|2x|8.0.0b|
|VMware vCenter|1x|8.0.0b|
|Windows Server|1x|2022|
|Proxmox|1x|7.4.3|

## Virtuelle Infrastruktur

Da wir keine Enterprise Hardware zur Verfügung haben, sind wir darauf angewiesen, die Umgebung mit dem Nested Virtualization Konzept aufzubauen. Glücklicherweise besitzen wir einen ressourcenstarken Baremetal und mit dem Proxmox Hypervisor lässt sich die Umgebung ohne viel Probleme virtualisieren.

![virtual_infrastructure](../assets/models/virt_infra_vSolar.svg)

## Logische Topologie

```plantuml
@startuml
nwdiag {
    internet [shape = cloud];
    internet -- router;
    
        network 250-lab {
        address = "10.128.250.x/24"

        m226b-esxi-p01.vsolar.net [address = "10.128.250.30"];
        m226b-esxi-p02.vsolar.net [address = "10.128.250.30"];
        m226b-vcenter.vsolar.net [address = "10.128.250.x"];
        m226b-dc.vsolar.net [address = "10.128.250.59"];

        k01-vhost-p01.kocherhans.local [address = "10.128.250.254"];
        router [address = "10.128.250.1"];
    }

    network vsolar-internal-210 {
        m226b-esxi-p01.vsolar.net;
        m226b-esxi-p02.vsolar.net;
    }

    network vsolar-internal-220 {
        m226b-esxi-p01.vsolar.net;
        m226b-esxi-p02.vsolar.net;
    }
}
@enduml
```