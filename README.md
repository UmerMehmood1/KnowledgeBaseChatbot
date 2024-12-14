# Step-by-Step Guide for Setting Up DocCreatorMakeFont Project

This guide walks you through the process of setting up the **DocCreatorMakeFont** project with a custom Urdu font on an Ubuntu-based system using WSL (Windows Subsystem for Linux). It includes instructions to create the required files, configure dependencies, add a custom Urdu font, and build the project to generate the Urdu `.of` file.

## Step 1: Setting Up Ubuntu Using WSL
1. **Enable WSL on Windows**:
   - Open PowerShell as an administrator and run:
     ```powershell
     wsl --install
     ```
   - This installs Ubuntu as the default distribution.

2. **Set Up Ubuntu**:
   - After installation, launch Ubuntu from the Start menu.
   - Complete the initial setup by creating a user and password.

## Step 2: Create and Configure the `DocCreatorMakeFont.pro` File
1. **Navigate to the DocCreatorMakeFont Directory**:
   - In Ubuntu, create the project folder and navigate to it:
     ```bash
     mkdir ~/DocCreatorMakeFont
     cd ~/DocCreatorMakeFont
     ```

2. **Create the `DocCreatorMakeFont.pro` File**:
   - Inside the `DocCreatorMakeFont` folder, create a file named `DocCreatorMakeFont.pro` with the following content:
     ```pro
     # Define the project
     QT += core gui xml
     CONFIG += c++11
     TARGET = DocCreatorMakeFont
     TEMPLATE = app
     INCLUDEPATH += .
     SOURCES += main.cpp
     HEADERS += MainWindow.hpp
     LIBS += -lQt5Widgets
     MOC_DIR = ./.moc
     ```

3. **Configure the Project Using qmake**:
   - Run the following command to configure the project:
     ```bash
     qmake ./DocCreatorMakeFont.pro
     ```

## Step 3: Build the Project Using `rebuild.sh` Script
1. **Create the `rebuild.sh` Script**:
   - Inside the `DocCreatorMakeFont` directory, create the `rebuild.sh` script to automate the build process:
     ```bash
     #!/bin/bash
     qmake ./DocCreatorMakeFont.pro
     make clean
     make
     ./DocCreatorMakeFont -platform xcb
     ```

2. **Make the Script Executable**:
   - Run the following command to make the `rebuild.sh` script executable:
     ```bash
     chmod +x rebuild.sh
     ```

3. **Run the Build Process**:
   - Execute the script to rebuild and run the application:
     ```bash
     ./rebuild.sh
     ```

## Step 4: Add Custom Urdu Font to the Project
1. **Create a New Directory for Fonts**:
   - Inside the `DocCreatorMakeFont` project folder, create a `fonts` directory:
     ```bash
     mkdir fonts
     ```

2. **Add the Urdu Font File**:
   - Place your **Urdu font** file (e.g., `urdu_font.ttf`) in the `fonts` directory.

3. **Modify the Code to Load the Font**:
   - Open the appropriate file (e.g., `MainWindow.cpp`) and add the following code to load the custom Urdu font:
     ```cpp
     QString currentDir = QDir::currentPath();
     QString fontPath = currentDir + "/fonts/urdu_font.ttf";
     int fontId = QFontDatabase::addApplicationFont(fontPath);
     QStringList fontFamilies = QFontDatabase::applicationFontFamilies(fontId);
     if (!fontFamilies.isEmpty()) {
         QString urduFontFamily = fontFamilies.at(0);
         QFont urduFont(urduFontFamily);
         fontLabel->setFont(urduFont);
         sizeLabel->setFont(urduFont);
         m_processPB->setFont(urduFont);
     }
     ```

## Step 5: Generate the Urdu `.of` File
   - After modifying the code, run the `rebuild.sh` script to rebuild the project and generate the Urdu `.of` file.

## Step 6: Place the Urdu `.of` File in the Correct Directory
1. **Move the Urdu `.of` File**:
   - Once the `.of` file is generated, move it to the `DocCreator/data/font/` directory:
     ```bash
     mv urdu.of ~/DocCreator/data/font/
     ```

## Step 7: Create Custom Keyboard for Urdu
1. **Navigate to the Keyboards Directory**:
   - Go to the `keyboards` directory inside the `DocCreator` project:
     ```bash
     cd ~/DocCreator/keyboards
     ```

2. **Create a Custom Keyboard for Urdu**:
   - Implement the custom keyboard layout for Urdu input by modifying the keyboard files.

## Step 8: Final Testing
1. **Run the DocCreator Project**:
   - After completing all the steps, run the project to test the functionality of the custom Urdu font and the Urdu keyboard:
     ```bash
     ~/installed/DocCreator/bin/DocCreator
     ```

2. **Verify the Font and Keyboard**:
   - Ensure that the Urdu font is properly loaded and applied to the labels.
   - Verify that the custom Urdu keyboard works as expected.

## Conclusion
You have successfully set up the **DocCreatorMakeFont** project, added a custom Urdu font, and created the corresponding `.of` file. Additionally, you have implemented a custom keyboard for Urdu input in your application.

