# Threat Modeling Tool Extension for Penetration Tester (TMTe4PT)

## About
This tool was designed as an extension for the [Microsoft Threat Modeling Tool (TMT)](https://www.microsoft.com/en-us/download/details.aspx?id=49168) with an adapted version of the automotive threat modeling template from the [NCC Group](https://www.nccgroup.trust/uk/about-us/newsroom-and-events/blogs/2016/july/the-automotive-threat-modeling-template/).

The extension adds the calculation of the [Common Vulnerability Scoring System v2](https://www.first.org/cvss/v2/guide) for each threat generated by the TMT, as well as the option of bulk modifications.
With the filtering and sorting of the tabular data, it is possible for a penetration tester to find the threats which match his/her criteria and therefore supports in prioritizing test activities (e.g., a tester wants to find the threats with the highest CVSS Score with *Ethernet* as
*Interaction* and a *Local* (L) *Access Vector*).

## Features
* Adding CVSS calculation to threats generated by the Microsoft TMT 2016
* Import of the *save* (.tm7) and *report* (.htm) file of the TMT
* Filtering the threats with a simple query logic
* Bulk modification of threats
* Export of the data and/or query in json format
* **For more information about the background and methodology see the blog post [Threat Modeling with TMTe4PT](https://www.schutzwerk.com/en/43/posts/tmte4pt/)**

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build static HTML file in the dist folder
npm run static
```

### Docker

There's a pre-built Docker image for this project. Please refer to the `docker` folder for more information.

## Usage
For easy usage, a static HTML file can be build and used in any modern browser without using a webserver or any other files. All JS and CSS code is bundled inside the HTML file.
This *index.html* file (in *dist* folder) can be generated with `npm run static`. A version from December 2018 can be found in 'tmte4pt/testFiles/TMTe4PT.html' for fast testing.

Otherwise, the typical tools can be used. For example, launch the application with `npm run dev`, or build it with `npm run build` and run the files from the *dist* folder in a web server (e.g. `python -m SimpleHTTPServer` in the *dist* folder).

**Quick Start**
![Screenshot of HomePage](/ReadMeImages/HomePage.png "Home Page of TMTe4PT")

After the application is started, the report file of the Microsoft Threat Modeling Tool (Example file at `tmte4pt/testFiles/ACC_Report.htm`) and the save file (Example file at `tmte4pt/testFiles/ACC.tm7`) can be loaded, using the appropriate buttons.

![Screenshot of Modify Threats](/ReadMeImages/ModifyThreats.png "Modify Threats Tab")
After that, the generated threats can be edited in the other tabs (e.g. Modify Threats).

![Screenshot of HowTo](/ReadMeImages/HelpHowTo.png "HowTo Help Page")
For a detailed description about the usage of this tool, go to `Help -> HowTo`.

## Technical Details

### Technologies Used
The complete source code of the tool was written in plain JavaScript, but uses the framework [Vue.js (v2.5.2)](https://vuejs.org/v2/guide/) for building the single page application with the [Bootstrap-Vue (v2.0.0-rc.11)](https://bootstrap-vue.js.org/) and [Bootstrap (v4.1.3)](https://getbootstrap.com/docs/4.1/getting-started/introduction/) libraries for designing the UI.
With the help of [WebPack](https://webpack.js.org/) the code can be bundled into a static HTML5 file which can be opened by a web browser.

### Architecture
This project was created with `vue init webpack tmte4pt` (vue-cli 2.9.6) which predefined the structure of a typical node project, with an *index.html* and *package.json* file in the root folder, and the actual code in the *src* directory. The other files and folders are auto created by the build script of `vue init` and help with the project, like the eslint rules.

The code in the *src* folder is separated according to the vue guidelines. Everything used for rendering the view is bundled in a *.vue* file which can be reused among the project. Similarly, the JS logic and definition classes from the components were separated, so they may be reused in another JS Framework like React. The definition classes are only used to define types which import, store and convert data. These are simple ECMAScript 6 classes which contain only properties for the data and methods for importing or converting them.

All functional JS classes, which contain only logic (methods) and no properties are separated from the definition classes. Either primitive data types or the definition classes are used as arguments for the methods. Following the single responsibility principle (SRP), each distinct function has its own class.

### Execution Path
The TMTe4PTs' root file is the *index.html* file, where the compiled *app.js* will be referenced or included. The program start is from the *main.js* file where the Vue VM will be created referencing the root component *App.vue*. The app component references the navbar and the router-view component which contain the rest of the application. Additionally, the *State* will be created. This special object is used to store data between the components and therefore will be created at the startup and filled/modified during the execution of TMTe4PT. If the user is saving the data into a *.json* file, basically the state gets dumped into a file. If the user imports data, the state gets filled or overwritten.

### Implementing new Features
If a new feature will be added, a list of steps should be followed.

1. Add the component in the *components* folder as a *.vue* file, define a name and write the code.
2. Implement the JS code into the *classes* folder (or export it into this folder), and separate the logic from the data part.
3. If the component should be an extra page/tab, create a route in the *router.js* file. If needed, an entry may be added to the *Navbar.vue*.
4. If stuck or help is needed, read the documentation about [Bootstrap-Vue](https://bootstrap-vue.js.org/docs/components/) for the UI.

## Disclaimer
The tool was developed by Michael Wolf as part of the Master Thesis "Combining Safety and Security Threat Modeling to Improve Automotive Penetration Testing". The work was sponsored by the BMBF project [SecForCARs](https://www.forschung-it-sicherheit-kommunikationssysteme.de/projekte/sicherheit-fuer-vernetzte-autonome-fahrzeuge) and created at [SCHUTZWERK GmbH](https://www.schutzwerk.com/) (supervisor Dr. Bastian Könings) in cooperation with the [Institute of Distributed Systems](https://www.uni-ulm.de/en/in/vs/) at Ulm University (referee: Prof. Dr. Frank Kargl, supervisor Dr. Rens van der Heijden), and the [Institute of Energy Efficient Mobility](https://www.hs-karlsruhe.de/ieem/) at University of Applied Sciences Karlsruhe (co-referee: Prof. Dr. Reiner Kriesten, supervisor Jürgen Dürrwang and Florian Sommer).

## License

This project is licensed under the [GPLv3](https://www.gnu.org/licenses/gpl.txt).
