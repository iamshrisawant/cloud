# ✅ **SETUP FOR CLOUDSIM PROJECT (Clean, Practical, Command-Prompt Style)**

---

## **1. Check Java Installation (Already Done)**

```
PS C:\Users\Shriswarup Sawant> java -version
openjdk version "1.8.0_462"
OpenJDK Runtime Environment (Temurin)(build 1.8.0_462-b08)
OpenJDK 64-Bit Server VM (Temurin)(build 25.462-b08, mixed mode)

PS C:\Users\Shriswarup Sawant> javac -version
javac 1.8.0_462
```

➡️ **Java 8 is installed correctly. CloudSim 3.x requires Java 6–8 → Perfect setup.**

---

# **2. Download CloudSim**

### **Step 1 — Download the CloudSim ZIP**

Download **CloudSim 3.0.3** from GitHub/SourceForge.

ZIP contains files like:

```
cloudsim-3.0.3.jar
cloudsim-examples.jar
cloudsim-extensions.jar
commons-math3-3.2.jar
```

---

# **3. Create Project Folder**

Create a new folder anywhere, e.g.:

```
C:\CloudSimProject\
```

Inside it, create:

```
C:\CloudSimProject\lib\
C:\CloudSimProject\src\
```

Copy the downloaded JAR files into:

```
C:\CloudSimProject\lib\
```

---

# **4. Verify Folder Structure**

```
C:\CloudSimProject
│
├── lib
│   ├── cloudsim-3.0.3.jar
│   ├── cloudsim-examples.jar
│   ├── cloudsim-extensions.jar
│   └── commons-math3-3.2.jar
│
└── src
    └── Main.java      (we will create this file next)
```

---

# **5. Create Starter Program (Main.java)**

Create **Main.java** inside:

```
C:\CloudSimProject\src\
```

Paste this minimal CloudSim setup:

```java
import org.cloudbus.cloudsim.*;
import org.cloudbus.cloudsim.core.CloudSim;
import org.cloudbus.cloudsim.provisioners.*;

import java.util.*;

public class Main {

    public static void main(String[] args) {
        try {
            int users = 1;
            Calendar calendar = Calendar.getInstance();
            boolean traceFlag = false;

            CloudSim.init(users, calendar, traceFlag);

            Datacenter datacenter = createDatacenter("Datacenter_0");
            DatacenterBroker broker = new DatacenterBroker("Broker_1");

            System.out.println("CloudSim setup successful.");

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static Datacenter createDatacenter(String name) throws Exception {

        List<Pe> peList = new ArrayList<>();
        peList.add(new Pe(0, new PeProvisionerSimple(1000)));

        int hostId = 0;
        int ram = 8192;
        long storage = 100000;
        int bw = 10000;

        Host host = new Host(
                hostId,
                new RamProvisionerSimple(ram),
                new BwProvisionerSimple(bw),
                storage,
                peList,
                new VmSchedulerTimeShared(peList)
        );

        List<Host> hostList = new ArrayList<>();
        hostList.add(host);

        DatacenterCharacteristics characteristics = new DatacenterCharacteristics(
                "x86", "Linux", "Xen", hostList,
                10, 3.0, 0.05, 0.001, 0.0
        );

        return new Datacenter(name, characteristics,
                new VmAllocationPolicySimple(hostList), new LinkedList<>(), 0);
    }
}
```

---

# **6. Compile the Project (Command Prompt)**

Open Command Prompt inside the project folder:

```
PS C:\CloudSimProject> javac -cp "lib/*" src\Main.java
```

If successful → no errors.

---

# **7. Run the Project**

```
PS C:\CloudSimProject> java -cp "lib/*;src" Main
```

Expected output:

```
CloudSim setup successful.
```

---

# **8. Project Setup Completed ✔**

Now your system is ready for:

* Adding VMs
* Adding Cloudlets
* Implementing **Round Robin / Time-Shared / Space-Shared** scheduling
* Running full experiments