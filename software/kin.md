### Documentation for KIN

27-11-2001 JH Zhao  

This distribution includes a driver program and an executable program based on Morgan v2.0/kinship and Werner pedigree example. The remaining source code is available from (http://www.stat.washington.edu/thompson/Genepi/Morgan.shtml). Note Morgan v2.3 has kin program to handle for more sophisticated pedigree.

**A list of files**

kindrv.c       Driver  
kin.exe        Executable program  
readme        This file  
werner.pre  A pre-MakePed file  
werner.kin   Output for werner.pre

werner.pre is a sample input file in LINKAGE pre-MakePed format. Basically it includes family ID plus triple information of individual ID, father ID, and mother ID. If you wish to know more about that format, please consult Prof Jurg Ott's website at http://linkage.rockefeller.edu.

werner.kin was obtained from werner.pre by following command

kin werner.pre werner.kin

(or kin <werner.pre >werner.kin by redirection)  

Note Under Windows system this is only possible under MS-DOS prompt. To enter this mode, follow the sequence below,

Click Start -> Run, and type command, cd \your_kin_directory

The MS-DOS windows may be too small on WinNT, you can press Alt+Enter to toggle between high-resolution small window and low-resolution large window. You can switch between you Windows tasks using Alt+Tab.

If your system has MS-DOS Prompt in the Programs list, you can follow the sequence below,

Click Start -> Programs -> MS-DOS Prompt, cd \your_kin_directory

Otherwise the program automatically enters interactive mode, asking for pedigree ID, individual ID, father ID and mother ID. Like Morgan you will also need tell the program when your input ends by signalling EOF (end of file), i.e. ^Z for MS-DOS and ^D for Unix.

When you finish a MS-DOS session use exit command to give control back to Windows.

Cheers,

<center>Jing Hua Zhao  
Section of Genetic Epidemiology and Biostatistics  
Division of Psychological Medicine  
Institute of Psychiatry  
De Crespigny Park  
London SE5 8AF  
UK

<center>j.zhao@iop.kcl.ac.uk </center>

<center>Last modified: 14/1/2000 by: [Jing Hua Zhao](mailto:j.zhao@iop.kcl.ac.uk)</center>
