# Trends in OS
- Distribution and paralellisation
- Virtualization
- Customization
- User-Space
- Operating Systems in other domains and forms

# Distribution and paralellization
- **Barrelfish** is a research operating system from ETH Zurich and Microsoft Research
- Distributed Kernel by design
- Berrelfish treats each core as an independent Kernel
- There is no shared state, each core communicates with message passing
- Each core can be a different hardware architecture

- **Fuchsia** is an operating system from Google
- It targets  distrbuted IoT environments
- Unlike Linux it swaps signals for environment driven programming and is based around capabilities

# Virtualization
- Utility computing
- Co-hosting multiple virtual machines on a single node
![[image-39.png]]

## Mechanisms for virtualization
- Linux-based Virtual Machine acts as a virtualization layer
- Guest VMs hook into this
- Different OSs can be supported as guests
- Foundation of infrastructure-as-a-service cloud computing model

# Containerization
- Lightweight process-based isolation with a shared OS host kernel
- Not as flexible as heavyweight virtualization but much more efficient
- Popular for DevOps, serverless Deployment, etc.
- Key infrastructure: **DOCKER**
- Containers can be stored as binaries
- Scripts can compose containers in layers

## Supporting Containers
- Namespace isolation and resource limiting
- e.g. cgroups in Linux
- Processes run with isolated side-effects
- Like Linux chroot mechanism and FreeBSD jails

# Management and Network Orchestration
- **MANO** is like a distributed OS infrastructure by Could Systems, often used by telecommunication networks
- It is the mechanism to allocate resources
- It uses Policies to decide how
![[image-40.png]]

# Allocating System Resources
![[image-41.png]]

# What to optimise for
![[image-42.png]]

# All of the above:
- **Ensemble**
- Distributed language runtime and OS
- Everything is an **actor**
- Uses message passing and supports heterogeneous hardware via custom Virtual Machine
- Runtime self-adaptation via hot swapping
![[image-43.png]]

# Level 4/5 projects
![[image-44.png]]
Examples of previous projects (Paul): http://paul-harvey.org/people/alumn/