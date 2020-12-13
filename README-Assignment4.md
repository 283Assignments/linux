# Assignment 4

## Q1.

## Team members:
  - Mamatha Guntu
  - Arshia Sali
  
### Contribution: 
  Since this is analysis part, we both analyzed the exit count for each exit (with and without ept = 0) in our own VM setup and recorded the output and have provided the analysis at the end of this document.
    
### Procedure:
  - Run assignment 3 code and boot a test VM using that code.
  - Once the VM has booted, recorded total exit count information (total count for each type of exit handled by KVM).
  - Shutdown your test (inner) VM.
  - Remove the ‘kvm-intel’ module from your running kernel: rmmod kvm-intel
  - Reload the kvm-intel module with the parameter ept=0 (this will disable nested paging and force KVM to use shadow paging instead)
  - insmod /lib/modules/XXX/kernel/arch/x86/kvm/kvm-intel.ko ept=0
  - Boot the same test VM again, and capture the same output as in step 2.
## Q2.

### Without EPT
## OUTPUT SCREENSHOT

![1WithoutEPT](https://user-images.githubusercontent.com/37695314/102023812-d116cb80-3d42-11eb-98d3-fbe5039a32bf.png)

![2WithoutEPT](https://user-images.githubusercontent.com/37695314/102023821-dc69f700-3d42-11eb-8ff4-14fe2333aff0.png)

## With EPT
## OUTPUT SCREENSHOT

![2WithEpt](https://user-images.githubusercontent.com/37695314/102023848-06bbb480-3d43-11eb-82b5-7a20708d1db6.png)

![1WithEpt](https://user-images.githubusercontent.com/37695314/102023840-f73c6b80-3d42-11eb-8c0f-7af6ae7c79c0.png)

## Q3.
    
### Analysis of Exits
    The exit count was found to increase with ept=0 (Shadow Paging) when compared to Nested Paging.
    This was in accordance with the expected behaviour as Shadow Paging involves Loading and Reading from CR3 registers for each instruction, TLB Flush Exit, INVLPG, INVPCID exits.
    
    
## Q4.
    
### Difference in WithoutEPT / With EPT
      Without EPT (Nested Paging), exit count for INVLPG and INVPCID is zero whereas With EPT=0 (Shadow Paging) we have a huge exit count for these exit reasons as it involves TLB Flush on each CR3 update.
      Also, the exit counts are found to be increasing for Shadow Paging.
