## Introduction 

---

#### The Story

It seemed as the sun rose this morning, it had already been decided that today would be another day of chaos in Wareville. At least that’s the feeling all the folks at “DoorDasher” got. DoorDasher is Warevilles local food delivery site, a favourite of the workers in The Best Festival Company, especially on long days when they get home from work and just can’t bring themself to make dinner. We’ve all been there, I’m sure.

Well, one Wareville resident was feeling particularly tired this morning and so decided to order breakfast. Only to find King Malhare and his bandit bunny battalions had seized control of yet another festive favourite. DoorDasher had been completely rebranded as Hopperoo. All of the ware’s favourite dishes had been changed as well. Reports started flooding into the DoorDasher call centre. And not just from customers. The health and safety food org was on the line too, utterly panicked. Apparently, multiple Wareville residents were choking on what turned out to be fragments of Santa’s beard. Wareville authorities were left tangled in confusion today as Hopperoo faced mounting backlash over reports of “culinary impersonation.” Customers across the region claim to have been served what appears to be authentic strands of Santa’s beard in place of traditional noodles.

A spokesperson for the Health & Safety Food Bureau confirmed that several diners required “gentle untangling” and one incident involved a customer “achieving accidental facial hair synchronisation.”

Beardgate news coverage

Immediately, one of the security engineers managed to log on and make a script that would restore DoorDasher to its original state, but just before he was able to run it, Sir CarrotBaine caught wind of his attempt and locked him out of the system. All was lost, until the SOC team realised they still had access to the system via their monitoring pod, an uptime checker for the site. Your job? As a SOC team member of DoorDasher, can you escape the container and escalate your privileges so you can finish what your team started and save the site!

#### Learning Objectives
- Learn how containers and Docker work, including images, layers, and the container engine
- Explore Docker runtime concepts (sockets, daemon API) and common container escape/privilege-escalation vectors
- Apply these skills to investigate image layers, escape a container, escalate privileges, and restore the DoorDasher service
- DO NOT order “Santa's Beard Pasta”

---

## Walkthrough 

**01 Containers**

Containers were created to solve common problems that arise when deploying modern applications. Applications today often depend on specific versions of libraries, runtimes, and system configurations, which can make installation difficult across different environments. Troubleshooting also becomes more complex when it is unclear whether an issue is caused by the application itself or by the system it is running on. In addition, running multiple applications or different versions of the same application on one machine can lead to dependency conflicts.

Containerisation addresses these challenges by isolating applications along with everything they need to run into a single, self-contained unit called a container. Each container includes the application code, libraries, and dependencies, ensuring it behaves the same regardless of where it is deployed. Because containers share the host operating system’s kernel instead of running a full operating system, they remain lightweight and efficient while still providing strong isolation.

**02 Containers vs VMs**

Containers and virtual machines both provide isolation, but they do so in very different ways. A virtual machine runs on top of a hypervisor and includes an entire guest operating system, along with its own kernel and system libraries. This makes virtual machines heavier in terms of storage and memory usage, but they offer strong isolation and the ability to run entirely different operating systems on the same physical host.

Containers, on the other hand, share the host operating system’s kernel and isolate only the application and its dependencies. This design makes containers much smaller, faster to start, and easier to manage at scale. While virtual machines are ideal for running legacy software or multiple operating systems, containers are better suited for modern application deployment where speed, portability, and efficiency are critical.

**03 Applications at Scale**

Modern application design has shifted from monolithic architectures to microservices-based architectures. In a monolithic model, the entire application is built and deployed as a single unit, meaning that scaling requires duplicating the whole application even if only one component is under heavy load. This approach can be inefficient and costly.

Microservices break an application into smaller, independent components based on business functions. Each component can be developed, deployed, and scaled independently. Containers play a key role in this approach because their lightweight nature allows individual services to be quickly replicated or replaced as demand increases. This makes containers the preferred technology for scalable, cloud-native applications and explains their rapid adoption over the past decade.

**04 Docker**

Docker is one of the most widely used container engines and is responsible for popularising container technology. It is an open-source platform that allows developers to build, package, deploy, and manage containers in a consistent and repeatable way. Docker uses Dockerfiles, which are simple text-based scripts that define the application environment, dependencies, and configuration needed to run a service.

By using Docker, developers can ensure that applications behave the same in development, testing, and production environments. Docker interacts directly with the host operating system’s kernel to provide isolation while remaining lightweight. Because of its simplicity, efficiency, and strong ecosystem, Docker has become the container engine of choice for many organisations, including DoorDasher, where it is used for building and running containerised services.

**05 Escape Attack & Sockets**

Although containers provide isolation, they are not immune to security risks. A container escape occurs when a process running inside a container gains access to the host system or other containers, breaking out of its intended isolation boundary. This can happen due to misconfigurations, vulnerabilities, or overly permissive container settings, such as running containers in privileged mode.

Containers rely on a client-server architecture, where command-line tools act as clients and communicate with the container runtime daemon through an API, often exposed via Unix sockets. If an attacker inside a container can access this runtime socket, they may be able to issue commands to the container engine directly. This could allow them to create new privileged containers, access the host system, or bypass network restrictions. Understanding how container sockets work is therefore critical for securing containerised environments and preventing escape attacks.

---

## Objectives 

What exact command lists running Docker containers?

Command: `docker ps`

<img width="1326" height="338" alt="image" src="https://github.com/user-attachments/assets/b5c8a9db-3ef2-4416-9b3d-686935a43e95" />

What file is used to define the instructions for building a Docker image?

<img width="390" height="75" alt="image" src="https://github.com/user-attachments/assets/fdd496c2-ac2b-4bf3-a944-5acc59c49e21" />

`Dockerfile`

What's the flag?

```
docker ps 

docker exec -it uptime-checker sh

ls -la /var/run/docker.sock

docker ps 
```

<img width="1331" height="746" alt="image" src="https://github.com/user-attachments/assets/be4de0c8-6207-4336-8351-2fe72de7386f" />

```
docker exec -it deployer bash

whoami

sudo /recovery_script.sh
```

<img width="1322" height="373" alt="image" src="https://github.com/user-attachments/assets/d881db8e-4c09-457a-ace1-2eacae8dabc8" />

```
ls -a 

cd .. 

cat flag.txt 
```

<img width="1315" height="242" alt="image" src="https://github.com/user-attachments/assets/edb31deb-9144-48ae-9a32-19ae66f4df08" />

`THM{DOCKER_ESCAPE_SUCCESS}`


Bonus Question: There is a secret code contained within the news site running on port 5002; this code also happens to be the password for the deployer user! They should definitely change their password. Can you find it?

<img width="1348" height="775" alt="image" src="https://github.com/user-attachments/assets/1c8a9e4b-c13a-4371-9785-716ee6587cbd" />

<img width="1353" height="783" alt="image" src="https://github.com/user-attachments/assets/ade00d20-789a-48b6-b45c-b1d6eed531f4" />

This words in different font color seems suspicious 

`DeployMaster2025!`

---

## Key Takeaways 

- Containers solve common deployment problems by packaging applications with their dependencies, ensuring consistent behavior across different environments.

- Unlike virtual machines, containers share the host operating system’s kernel, making them lightweight, fast to start, and efficient to scale.

- Containers are a core enabler of microservices architectures, allowing individual application components to be deployed and scaled independently.

- Docker simplifies container creation and management through Dockerfiles, images, and a standardized container engine workflow.

- Container isolation is not absolute; misconfigurations and excessive privileges can introduce serious security risks.

- Access to the Docker runtime socket (docker.sock) is a critical security concern, as it can allow attackers to control the container engine.

- Container escape techniques can be used to escalate privileges from within a container to the host system.

- Proper monitoring and least-privilege configurations are essential to securing containerized environments.

---

## Reflection

This exercise highlighted how technologies designed to improve efficiency and scalability can become powerful attack vectors when misconfigured. By working through the container escape scenario, it became clear that understanding Docker’s internal architecture is just as important as knowing how to deploy applications with it. The ability to access the Docker socket from within a container demonstrated how quickly isolation can break down when security boundaries are not enforced. Recovering the DoorDasher service reinforced the importance of defensive thinking, where awareness of container internals, privilege management, and runtime exposure plays a crucial role. Overall, this challenge strengthened my understanding of container security and emphasized that effective defense requires both operational knowledge and a strong security mindset.


