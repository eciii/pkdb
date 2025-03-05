#Linux_Technology

Full virtualization

- The goal is: create a virtual environment that emulates a physical machine in such a way that an OS can be executed within that environment without it ever noticing that it is being executed in a virtual environment instead of a physical one.
- Part of the goal is to run _unmodified_ OSes.
- It's all about deception.

Full Paravirtualization

- The goal is: implement an interface that allows OSes to interact with the host for everything they need.
- Thus no emulated hardware environments are needed.
- Thus only _modified_ OSes can run on such host.
- It's all about cooperation.


VMware implemented _full virtualization_ in 1998
##### Sensible and privileged machine instructions

A machine instruction is **sensible** if its behavior when executed in kernel-mode is different than its behavior when executed in user-mode.

A machine instruction is **privileged** if it should only be executed in kernel-mode. The machine can take several approaches when it encounters such an instruction in user-mode:

- It can silently ignore the instruction.
- It can partially execute the instruction. For example the x86 POPF instruction fully modifies the FLAGS register in kernel-mode but in user-mode the Interrupt Flag is always left unmodified.
- It can generate an exception (fault or abort; remember that exceptions are sometimes called synchronous or software interrupts). In this case an exception handler is executed, and, if the exception is a fault, then the "privileged" instruction is retried.
- The machine can "trap to the kernel" and let the kernel decide what to do. Once the trap handler is done the execution of the original code resumes.

**Fact:** All privileged instructions are also sensible. This is because privileged instructions are always handled differently in user-mode.

##### Virtualization and the virtual sensitivity problem

Virtualization is achieved once the following to qualities are met (these qualities were stated by Popek and Goldberg in a [seminal paper](https://dl.acm.org/doi/pdf/10.1145/361011.361073) in 1974):

- **Controlled isolation:** The host has full control of the virtual environments where the guests run and ensures that the guests are not able to affect each other or the host.
- **Fidelity:** The behavior of a guest running in a virtual machine should be identical to that of the same guest running in a physical machine with the same characteristics.

Virtual machines always run in user-mode, so the host machine needs to emulate the kernel-mode/user-mode split for each guest. These two emulated modes are called **virtual kernel-mode** and **virtual user-mode**.

- If an instruction is not sensible then it doesn't matter on which mode it is executed since the behavior is the same in both modes. Thus fidelity is preserved in this case.
- If an instruction is sensible and it is being executed in virtual user-mode then it can be safely executed as-is because anyway it will be executed in (actual) user-mode. Thus fidelity is also preserved in this case.
- If an instruction is sensible and it is being executed in virtual kernel-mode then fidelity cannot be preserved: the guest kernel expects the behavior of the instruction as if it was being executed in kernel-mode but the instruction behaves differently because it is actually being executed in user-mode. I call this problem the **virtual sensitivity problem**.

To address the virtual sensitivity problem it is necessary to find a way to change the behavior of sensible instructions when executed in virtual kernel-mode. Such change has to:

- do whatever the guest kernel expects (to preserve fidelity)
- do it in a controlled manner that only affects the guest (to preserve isolation)

##### The Popek-Goldberg condition

A machine conforms to the **Popek-Goldberg condition** when all the sensible instructions of the machine always "trap to the kernel" when executed in user-mode. A **Popek-Goldberg machine** is a machine that satisfies the Popek-Goldberg condition.

**Fact:** In a Popek-Goldberg machine all privileged instructions always "trap to the kernel" when executed in user-mode.

**Fact:** There is an easy solution to the virtual sensitivity problem in Popek-Goldberg machines. The reason is because in such machines, if a sensible instruction is executed by the guest (which runs in user-mode) then the instruction always traps to the host kernel and thus the following procedure can carried out:

- If the host kernel determines that the sensible instruction was executed in virtual user-mode then execution can be forwarded to the corresponding trap handler in the guest kernel.
- If the host kernel determines that the sensible instruction was executed in virtual kernel-mode then is in the position to _emulate_ whatever the guest kernel expects and _emulate_ it in a controlled manner that only affects the guest. In this case the problem becomes just an "implementation detail".

##### The trap-and-emulate technique

The solution to the virtual sensitivity problem in Popek-Goldberg machines is the perfect example of a technique known as **the trap-and-emulate** technique. The idea is to always trap to the kernel when a sensible instruction is executed in virtual kernel mode and emulate the behavior of such instruction in a controlled manner that only affects the guest.

The question now is if it is possible to implement this technique in machines that do not conform to the Popek-Goldberg condition (such as x86 machines). It turns out that this is possible using **(dynamic) binary translation**. The main idea is to dynamically scan the instructions that are going to be executed by the guest in virtual kernel-mode and then convert the sensible instructions into traps before they are executed.

---

**hypervisor** / **virtual machine monitor (VMM)**

Tanenbaum's "Modern Operating Systems" give the same meaning to both concepts (the software in charge of all the tasks related to virtualization). More recently there seems to be a split between both concepts:

- The hypervisor is the part of the software that actually _supervises_ the the execution of the guest OS in order to provide the illusion of _isolation_ and _control_. The name _hypervisor_ means, in fact, supervisor of supervisors, a characterization that makes sense because the guest OSes can be considered themselves supervisors of their own processes. \[trap and emulate]
- 

**Landscape**

A **hypervisor** is a piece of software that is able to _supervise_ the concurrent execution of several OSes. Since an OS _supervises_ the concurrent execution of several processes, a hypervisor can be considered as a supervisor of supervisors, hence the name _hypervisor_.

When an OS runs directly on hardware it runs with full privileges, i.e it can access all the resources provided by the hardware and execute all the instructions