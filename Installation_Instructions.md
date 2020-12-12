# Setup instructions for RNA-Seq Demystified

This document guides you through the installation of software necessary to participate in the RNA-Seq Demystified workshop. To complete this setup, you will need:

- A Macintosh or Windows workstation connected to the internet.
- Administrative privilieges on this workstation.
- The server address of the workshop Linux environment (supplied to you by the workshop hosts) .
- Your individual username and password (supplied to you by the workshop hosts).
- About 30 minutes (~20 minutes unattended).


#### Table of Contents

- Introduction
  - How to get help


- [Windows setup](#windows-setup)
  - Zoom 
  - Windows PowerShell / Git-Bash
  - Connecting to a Linux environment
  - Installing R/RStudio
  - Notes


- [Macintosh setup](#macintosh-setup)
  - Zoom
  - Terminal
  - Connecting to a Linux environment
  - Installing R/RStudio
  - Notes


## Introduction

- The workshop will be conducted over Zoom, Linux (first day), and R with RStudio (second day). 
  - The workshop exercises are designed to work well for Windows or Mac users and the installation 
    instructions are detailed in separate sections below.
  - *If you do not have Administrative privileges it will be tricky to 
    install R/RStudio; you may need to coordinate with your System Admin/IT Support team to get 
    these installed.* See **How to get Help** below if you need more guidance on this.
  - To ensure a consistent experience, we have created a temporary shared Linux environment 
    and established login/passwords that will be communicated to participants individually. 
    Please note that this environment is optimized for the exercises in this particular workshop but is 
    likely unsuitable for analyzing your specific datasets; in particular:
    - It is not sized for compute or storage intensive operations.
    - It is not secured for sensitive data of any kind.
    - This environment is temporary and will be removed shortly after the conclusion of the workshop. 
      (The workshop will breifly cover alternative compute/storage options.)

### How to get help

While we have endeavored to make this setup process work robust and comprehensive, installing bioinformatics software is tricky and we would be happy to lend a hand to get things working. 

- If you have problems/questions, please don't hesitate to email us at: bioinformatics-workshops@umich.edu
- When emailing it will speed things along if you could include:
  - Whether you are using Windows or Mac (and optionally which version of the OS you are using).
  - Whether you have Administrative privileges on your workstation.
  - The specific text of any error messages, if applicable. 
  

---

## Windows setup

### Zoom

  If you have not used Zoom before, please use the following link to install "Zoom client for Meetings"
  https://zoom.us/download
  
### Windows PowerShell / git-bash
  
  Windows PowerShell, git-bash, and Windows Subsystem for Linux (WSL) are programs that enable you to 
  connect to our workshop Linux environment. WSL is a powerful option, but can be 
  difficult to install/configure. If you have WSL installed and you are comfortable using it, you can 
  use that and skip to the next section. 
  
  PowerShell is built into Windows and is simpler to use, but the bits we need only work on later 
  Windows versions that have been correctly configured; Git-bash is a bit more involved to setup but 
  works on almost all versions. Start by seeing if Powershell works on your workstation:

  1. Press Windows+R keys to open the **Run** dialog; type **"powershell"** in the text box and hit 
     enter. This will launch a new PowerShell window.
  2. In the PowerShell window, you will see a prompt that looks something like this: 
     
     `PS C:\Users\your_username>`
     
     Type the following command at the prompt and hit enter.
     
     `where.exe ssh` 
     
     The PowerShell will print a new line. If if prints a single line that ends with ssh.exe (for 
     example, `C:\Windows\System32\OpenSSH\ssh.exe`), then your PowerShell is correctly configured and 
     you can proceed to the next section: **Connecting to a Linux environment**.
     
     If the PowerShell returns something like `INFO: Could not find files for the given pattern(s)`, 
     then continue on below to install git-bash

  3. To download git-bash, in a web browser, go to:
     https://git-scm.com/downloads
     and click on **Windows**. Save and open the executable file to launch the installer.
 
  4. The git-bash installer presents *many* options; repeatedly click **Next** to simply accept all the 
     default options. At the end of this series of questions, you can click **Install**.
  
  5. The installer will show a progress bar; this should take less than a minute. *Note: you may see a
     warning towards the end "Unable to run post-install scripts; no output?"; this will not impact 
     your installation and you can ignore it by clicking **Ok**.*

  6. On the final screen (Completeing the Git Setup Wizard) you can uncheck **View Release Notes** 
     option and click **Finish**. The installer will close.
     
  7. To launch git-bash, from Start Menu, select "Git Bash"; this will create a new window with a 
     command prompt that looks something like this:

     `your_username@WSRTS001 MINGW64 ~
     $`

  8. Type the following command at the prompt and hit enter.
     
     `where ssh`

     git-bash will print one or more new lines. If the lines end with ssh.exe (for 
     example, `C:\Users\your_username\AppData\Local\Programs\Git\usr\bin\ssh.exe`), then git-bash is correctly 
     configured and you can proceed to the next section: **Connecting to a Linux environment**.

     If you can't install or launch git-bash, or if it returns an unexpected result, please see 
     **how to get help** for more assistance.


### Connecting to a Linux environment (Windows)

  1. Using either the PowerShell or git-bash command window you launched above, type the following 
    command, replacing the YOUR_USERNAME with the username supplied to you by the workshop hosts. 
    Hit enter to execute the command. *Note: you can copy the command below to the clipboard and then 
    right-click in the command window to paste.*
    
    `ssh YOUR_USERNAME@SERVER_ADDRESS`

    The first time you run this command, you may see a prompt like the following; hit the Enter key to continue. 

            The authenticity of host '...SERVER_ADDRESS...' can't be established.
            ECDSA key fingerprint is SHA256:izeVPFh3fZEFP....
            Are you sure you want to continue connecting (yes/no/[fingerprint])? yes


  2. The command will print a warning (e.g. *Warning: Permanently added 
    'SERVER_ADDRESS' (ECDSA) to the list of known hosts*). This is fine.
  
  3. It will prompt you for your password. Type the password supplied by the workshop hosts. 
    *(Note this password is case sensitive.)* If you successfully logged in, the command window 
    should show something like this:

```
        ------------------------------
        Welcome to RNA-Seq Demystified
        ------------------------------
        Last login: Wed Dec  9 17:37:15 2020 from 123...
        (workshop) YOUR_USER_NAME@ip:~$ 
```

   4. If you see the message above, you can close this window (or type *exit*) and proceed to the next 
      section. If you are *not* able to login, please see the **How to get help** section above for 
      more assistance.

#### Installing R/RStudio (Windows)

  The simplest way to install RStudio in Windows requires Administrative privileges on your workstation. *If you do 
  not have Administrative privileges, you may need to coordinate with your System Administrator/IT Support Team to get 
  RStudio installed on your workstation.*
  See **How to Get Help** above to contact us for guidance on get RStudio working.
  
  1. RStudio depends on the R programming environment, so we have to install that first. 
     In a web browser, open: 
     
     https://cran.rstudio.com/bin/windows/base/ 
     
     and click "Download R 4.0.3 for Windows" (the version may be slightly different). Open the downloaded 
     executable to launch the R installer.

  2. The installer will walk through several options; accept all the defaults (by repeatedly clicking **Next**)
     and when prompted, click **Install**. The installer will show a progress bar; the process takes about 2 minutes;
     click **Finish** when prompted to close the installer.

  3. To install RStudio, in a web-browser, open https://rstudio.com/products/rstudio/download/#download
     and click on **Download RStudio Desktop for Windows**.  Open the downloaded executable to launch the 
     installer.
       
  4. The installer will either prompt you to login as an Admin user or (if your current account has Admin privileges) 
     simply ask you to allow it to make changes. Click **Yes**
    
  5. The installer will walk through several options; accept all the defaults (by repeatedly clicking **Next**) 
     and when prompted, click **Install**. The installer will show a progress bar; the process takes less than one 
     minute; click **Finish** when prompted to close the installer.
       
  6. Press Windows+R keys to open the **Run** dialog; type **"RStudio"** in the text box and hit 
     enter. This will launch a new RStudio window. The RStudio window is divided into several panes. The lower left 
     pane shows the **Console** tab and will show some text followed by a command prompt (>):

            R version 4.0.3 (2020-10-10) -- "Bunny-Wunnies Freak Out"
            Copyright (C) 2020 The R Foundation for Statistical Computing
            Platform: x86_64-w64-mingw32/x64 (64-bit)

            R is free software and comes with ABSOLUTELY NO WARRANTY.
            You are welcome to redistribute it under certain conditions.
            Type 'license()' or 'licence()' for distribution details.

              Natural language support but running in an English locale

            R is a collaborative project with many contributors.
            Type 'contributors()' for more information and
            'citation()' on how to cite R or R packages in publications.

            Type 'demo()' for some demos, 'help()' for on-line help, or
            'help.start()' for an HTML browser interface to help.
            Type 'q()' to quit R.

            >


   7. The workshop exercises requires the installation of special R libraries. To install them into RStudio, copy the block of 
      text below, paste into the RStudio **Console** tab, and press **Enter** to execute. *(Note: These installations automatically
      trigger the installation of a litany of dependent libraries so you will see repeated progress bars and code flying by 
      in the Console window. This step takes about 15 minutes, so now is a good time to get coffee/tea while RStudio cooks.)*

            install.packages("tidyr")
            install.packages("ggplot2")
            install.packages("pheatmap")
            install.packages("ggrepel")
            install.packages("formattable")
            install.packages("RColorBrewer")
            if (!requireNamespace("BiocManager", quietly = TRUE))
                install.packages("BiocManager")
            BiocManager::install("biomaRt", ask=FALSE)
            BiocManager::install("DESeq2", ask=FALSE)
            missing <- setdiff(c("tidyr", "ggplot2", "pheatmap", "ggrepel", "formattable", "RColorBrewer", "biomaRt", "DESeq2"), rownames(installed.packages()))
            if (!length(missing)) { cat("Ready for RNA-Seq Demystified\n")} else {cat("PROBLEM: could not install:", missing, "\n")}


   8. The Console output should conclude with the text `Ready for RNA-Seq Demystified`.
   
   9. Press **Control-Q** close RStudio; when prompted to *Save workspace image...*, click **Don't Save**.
    
      If you had problems installing R/RStudio or installing the R libraries above, please see the **How to get help** section above for 
      more assistance.
   
   
#### *Thank you for patience and fortitude. Your workstation is ready for the workshop.*
   
### Notes (Windows)

- Following the workshop, you can remove any of Git-bash, R and RStudio. As an Admin user, go Start > Settings > Apps 
  & Features. Click on the program to remove and click Uninstall. 


---

## Macintosh setup

### Zoom

  1. If you have not used Zoom before, please use the following link to install "Zoom client for Meetings"
  https://zoom.us/download
       

  2. To enable screen sharing (useful for breakout rooms and tech support)
       1. System Preferences >> Security & Privacy: click on the Privacy tab
       2. Select **Screen Recording** on left tab
       3. Scroll to the bottom of the right tab and verify **Zoom** is checked.
      

  3. To enable remote control (useful for breakout rooms and tech support)
       1. System Preferences >> Security & Privacy: click on the Privacy tab
       2. Select **Accessibility** on left tab
       3. Scroll to the bottom on the righ tab and verify **Zoom** is checked.
       4. If it's not checked, click the lower left lock icon and enter user password when prompted. Then check Zoom. Then click the lock again.

### Terminal

  1. Macintosh has a built in command window called Terminal. Press Command + Space to launch Spotlight. In the search field, type "Terminal" and double-click on the top result. 
  
  You will see a new Terminal window containing something like this (your may have more text and the last line may look a bit different; that's ok)


        Last login: Thu Dec 10 12:44:03 on ttys003
        MacBook: ~ your_username$

  2. At the command prompt, type `date` and press **Return**. The Terminal should print the current date and time.

### Connecting to a Linux environment (Macintosh)

  1. Using the Terminal window you launched above, type the following 
    command, replacing the YOUR_USERNAME with the username supplied to you by the workshop hosts. 
    Hit enter to execute the command. *Note: you can copy the command below to the clipboard and then 
    right-click in the command window to paste.*
    
    `ssh YOUR_USERNAME@SERVER_ADDRESS`

    The first time you run this command, you may see a prompt like the following; hit the Enter key to continue. 

            The authenticity of host '...SERVER_ADDRESS...' can't be established.
            ECDSA key fingerprint is SHA256:izeVPFh3fZEFP....
            Are you sure you want to continue connecting (yes/no/[fingerprint])? yes


  2. The command will print a warning (e.g. *Warning: Permanently added 
    'SERVER_ADDRESS' (ECDSA) to the list of known hosts*). This is fine.
  
  3. It will prompt you for your password. Type the password supplied by the workshop hosts. 
    *(Note this password is case sensitive.)* If you successfully logged in, the command window 
    should show something like this:

```
        ------------------------------
        Welcome to RNA-Seq Demystified
        ------------------------------
        Last login: Wed Dec  9 17:37:15 2020 from 123...
        (workshop) YOUR_USER_NAME@ip:~$ 
```

   4. If you see the message above, you can close this window (or type *exit*) and proceed to the next 
      section. If you are *not* able to login, please see the **How to get help** section above for 
      more assistance.


### Installing R/RStudio (Macintosh)


  1. RStudio depends on the R programming environment, so we have to install that first. 
     In a web browser, open: 
     
     https://cran.rstudio.com/bin/macosx/ 
     
     and click the link "R-4.0.3.pkg" (the version may be slightly different). Open the downloaded 
     executable to launch the R installer.

  2. The installer will walk through several options; accept all the defaults (by repeatedly clicking **Continue**)
     and when prompted, click **Install**. The installer will prompt you to confirm your username/password.
     The installer will show a progress bar; the process takes about 1 minutes;
     click **Finish** when prompted to close the installer.

  3. To install RStudio, in a web-browser, open:
    
       https://rstudio.com/products/rstudio/download/#download
       
     and click on **Download RStudio Desktop for Mac**.
       
  4. Opening the downloaded executable opens a window with "RStudio" and your Applications folder icons.  Drag the RStudio into the Applications folder. 
     (If you see a dialog that claims RStudio already exists, click **Keep Both**.) The installer will prompt you to confirm your username/password.
    
  5. The installer will walk through several options; accept all the defaults (by repeatedly clicking **Next**) 
     and when prompted, click **Install**. The installer will show a progress bar; the process takes less than one minute.
    
  6. When completed, open the Applications folder and double-click on the RStudio application. 
     You will see a dialog "*RStudio.app is an app downloaded from Internet. Are you sure you want to open it?*" Click **Open**.

     This will launch a new RStudio window. The RStudio window is divided into several panes. The lower left 
     pane shows the **Console** tab and will show some text followed by a command prompt (>):

            R version 4.0.3 (2020-10-10) -- "Bunny-Wunnies Freak Out"
            Copyright (C) 2020 The R Foundation for Statistical Computing
            Platform: x86_64-apple-darwin17.0 (64-bit)

            R is free software and comes with ABSOLUTELY NO WARRANTY.
            You are welcome to redistribute it under certain conditions.
            Type 'license()' or 'licence()' for distribution details.

              Natural language support but running in an English locale

            R is a collaborative project with many contributors.
            Type 'contributors()' for more information and
            'citation()' on how to cite R or R packages in publications.

            Type 'demo()' for some demos, 'help()' for on-line help, or
            'help.start()' for an HTML browser interface to help.
            Type 'q()' to quit R.

            >


   7. The workshop exercises requires the installation of special R libraries. To install them into RStudio, copy the block of 
      text below, paste into the RStudio **Console** tab, and press **Enter** to execute. *(Note: These installations automatically
      trigger the installation of a litany of dependent libraries so you may see repeated progress bars and lots of code flying by 
      in the Console window. This step takes about 15 minutes, so now is a good time to get coffee/tea while RStudio cooks.)*

            install.packages("tidyr")
            install.packages("ggplot2")
            install.packages("pheatmap")
            install.packages("ggrepel")
            install.packages("formattable")
            install.packages("RColorBrewer")
            if (!requireNamespace("BiocManager", quietly = TRUE))
                install.packages("BiocManager")
            BiocManager::install(c("biomaRt","DESeq2"), update=FALSE, ask=FALSE)
            missing <- setdiff(c("tidyr", "ggplot2", "pheatmap", "ggrepel", "formattable", "RColorBrewer", "biomaRt", "DESeq2"), rownames(installed.packages()))
            if (!length(missing)) { cat("Ready for RNA-Seq Demystified\n")} else {cat("PROBLEM: could not install:", missing, "\n")}


   8. The Console output should conclude with the text `Ready for RNA-Seq Demystified`. 
   
   9. Press **Command-Q** close RStudio; when prompted to *Save workspace image...*, click **Don't Save**.
    
      If you had problems installing R/RStudio or installing the R libraries above, please see the **How to get help** section above for 
      more assistance.
   
   
#### *Thank you for patience and fortitude. Your workstation is ready for the workshop.*

### Notes (Macintosh)

- Following the workshop, you can remove any of R and RStudio. Open your Applications directory, and drag the R or RStudio application into the Trash.



---
