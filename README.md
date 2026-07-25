<h1 align="center">Wallcroft</h1>

<p align="center">
  <b>Driver Developer @ WarChill Team</b> &nbsp;•&nbsp; Systems &amp; Reverse Engineering &nbsp;•&nbsp; Offensive Security
</p>

<p align="center">
  <a href="https://github.com/WallcroftUK"><img src="https://img.shields.io/badge/GitHub-WallcroftUK-181717?style=flat-square&logo=github&logoColor=white"></a>
  <a href="#"><img src="https://img.shields.io/badge/Discord-WallcroftUK%231101-5865F2?style=flat-square&logo=discord&logoColor=white"></a>
</p>

---

Windows kernel-mode drivers, hypervisors (Intel VT-x / AMD SVM), game engine architecture, and
reverse engineering. Currently a Driver Developer at **WarChill Team**.

---

### 🔭 Active Projects

| Project | Stack | Role |
|---|---|---|
| **WarChill — Kernel & Hypervisor** | C, WDK, Intel VT-x, AMD SVM | Driver Dev |
| **Project Intoner** | C++, D3D12, ECS | Sole Dev |
| **NosQuest** | C#, custom server stack | Tech Lead |
| **Entwell (NewAge) — NosTale** | Delphi 12 | Internal Tools |

---

### ⚙️ Day-to-Day

<details>
<summary>AMD SVM — CPUID intercept at Ring -1</summary>

```c
void SvmHandleVmexit(PVCPU Vcpu, PVMCB Vmcb)
{
    switch (Vmcb->ControlArea.ExitCode)
    {
    case VMEXIT_CPUID:
    {
        int regs[4];
        __cpuidex(regs, (int)Vmcb->StateSave.Rax,
                        (int)Vmcb->StateSave.Rcx);
        if (Vmcb->StateSave.Rax == 1)
            regs[2] &= ~(1 << 31);
        Vmcb->StateSave.Rax = regs[0];
        Vmcb->StateSave.Rbx = regs[1];
        Vmcb->StateSave.Rcx = regs[2];
        Vmcb->StateSave.Rdx = regs[3];
        Vmcb->StateSave.Rip += 2;
        break;
    }
    case VMEXIT_MSR: SvmHandleMsr(Vcpu, Vmcb);           break;
    case VMEXIT_NPF: SvmHandleNestedPageFault(Vcpu, Vmcb); break;
    }
}
```
</details>

<details>
<summary>Intel VT-x — EPT 2 MB → 4 KB page split</summary>

```c
NTSTATUS EptSplitLargePage(PEPT_STATE State, ULONG64 PhysAddr)
{
    PEPT_PDE_2MB Large = EptGetPde2MB(State, PhysAddr);
    if (!Large || !Large->LargePage) return STATUS_NOT_FOUND;

    PEPT_PTE Pt = ExAllocatePool2(POOL_FLAG_NON_PAGED, PAGE_SIZE, 'tpEW');
    if (!Pt) return STATUS_INSUFFICIENT_RESOURCES;

    ULONG64 Base = Large->PageFrameNumber << 21;
    for (ULONG i = 0; i < 512; i++)
        Pt[i] = (EPT_PTE){ .Read=1, .Write=1, .Exec=1,
                           .PageFrameNumber = (Base >> 12) + i };

    ((PEPT_PDE)Large)->Value           = 0;
    ((PEPT_PDE)Large)->ReadAccess      = 1;
    ((PEPT_PDE)Large)->WriteAccess     = 1;
    ((PEPT_PDE)Large)->ExecuteAccess   = 1;
    ((PEPT_PDE)Large)->PageFrameNumber =
        MmGetPhysicalAddress(Pt).QuadPart >> 12;

    EptInveptSingleContext(State->EptPointer);
    return STATUS_SUCCESS;
}
```
</details>

---

### 🔐 Security Track Record

| CVEs | Full-chain compromises | Custom exploits |
|:---:|:---:|:---:|
| **187** | **31** | **57** |

---


### 🧠 Primary Stack
<code><img height="45" title="Delphi" src="https://img.shields.io/badge/Delphi-%23E32D2F.svg?style=for-the-badge&logo=delphi&logoColor=white"></code>
<code><img height="45" title="C++" src="https://raw.githubusercontent.com/github/explore/master/topics/cpp/cpp.png"></code> 
<code><img height="45" title="C#" src="https://raw.githubusercontent.com/github/explore/master/topics/csharp/csharp.png"></code> 
<code><img height="45" title="Assembly" src="https://raw.githubusercontent.com/github/explore/master/topics/assembly/assembly.png"></code>
<code><img height="45" title="Python" src="https://raw.githubusercontent.com/github/explore/master/topics/python/python.png"></code> 
<code><img height="45" title="Go" src="https://raw.githubusercontent.com/github/explore/master/topics/go/go.png"></code> 

---

### 🛠 Tools & Reverse Engineering
<code><img height="45" title="RAD Studio" src="https://img.shields.io/badge/RAD%23Studio-%23E32D2F.svg?style=for-the-badge&logo=embarcadero&logoColor=white"></code>
<code><img height="45" title="Ghidra" src="https://avatars.githubusercontent.com/u/48333017?v=4"></code>
<code><img height="45" title="x64dbg" src="https://avatars.githubusercontent.com/u/6233056?s=200&v=4"></code> 
<code><img height="45" title="Docker" src="https://raw.githubusercontent.com/github/explore/master/topics/docker/docker.png"></code> 
<code><img height="45" title="Kubernetes" src="https://raw.githubusercontent.com/github/explore/master/topics/kubernetes/kubernetes.png"></code>
<code><img height="45" title="Git" src="https://raw.githubusercontent.com/github/explore/master/topics/git/git.png"></code> 

---

### 📂 Archive

* **AetherHosting** — Cloud infrastructure and orchestration. `paused`
* **HackerWorld** — Security research and offensive tooling. `paused`

---

## 🌍 Social
<a href="https://github.com/WallcroftUK"><img src="https://img.shields.io/badge/-@WallcroftUK-%23181717?style=flat-square&logo=github" height="25"></a>
<a href="#"><img src="https://img.shields.io/badge/-WallcroftUK%231101-%232c2f33?style=flat-square&logo=discord" height="25"></a>
