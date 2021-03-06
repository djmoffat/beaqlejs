 BeaqleJS
=====================

Description
---------------------

BeaqleJS provides a framework to create browser based listening tests and is purely based on open web standards like HTML5 and Javascript. Therefore, the test runs in any modern web browser and allows an easy distribution of the test environment to a significant amount of participants in combination with simple configuration. Currently it supports ABX and MUSHRA style test procedures but can be easily extended to other test schemes.

To get a better impression about its functionality there are two demo test sites for the ABX and MUSHRA test classes:

* http://hsu-ant.github.io/beaqlejs/demo/abx/
* http://hsu-ant.github.io/beaqlejs/demo/mushra/

BeaqleJS has been presented at the [Linux Audio Conference 2014](http://lac.linuxaudio.org/2014/) at the ZKM in Karlsruhe, Germany. The [paper](http://lac.linuxaudio.org/2014/papers/26.pdf) and presentation [slides](http://lac.linuxaudio.org/2014/download/SKraft_BeaqleJS.pdf) are available from the conference archive.

If you want to cite BeaqleJS in your publications please use

    S. Kraft, U. Zölzer: "BeaqleJS: HTML5 and JavaScript based Framework for the Subjective Evaluation of Audio Quality", Linux Audio Conference, 2014, Karlsruhe, Germany

as a reference or link to our GitHub repository
    
    https://github.com/HSU-ANT/beaqlejs


Basic Setup
---------------------

1. Download the test scripts
   - you can either clone the git repository `git clone https://github.com/HSU-ANT/beaqlejs.git`
   - or simply download a zip of the current HEAD revision at `https://github.com/HSU-ANT/beaqlejs/archive/master.zip`
   - or choose an older release at `https://github.com/HSU-ANT/beaqlejs/releases`

2. Uncomment the line where the desired test class is initialized and loaded in the header of the `index.html` file.

        <script type="text/javascript">
            var testHandle;
            window.onload=function() {
                // Uncomment one of the following lines to choose the desired test class
                //testHandle = new MushraTest(TestConfig);  // <= MUSHRA test class
                //testHandle = new AbxTest(TestConfig);     // <= ABX test class
            };
        </script>

3. Prepare a config file and set its path in the prepared `<script></script>` tag in the header of the `index.html` file.

        <!-- load the test config file -->
            <script src="config/YOUR_CONFIG.js" type="text/javascript"></script>
        <!---->
    Two example config files for the MUSHRA and ABX test class are already supplied in the `config/` folder to serve as a starting point. Detailed information about the different test classes and configuration can be found below.


Test Configuration 
---------------------

### General Options ###

The available options can be dividided into a set of general options whitch apply to all test classes and other options, including file declarations, that are specific for a single test class.

    var TestConfig = {
        "TestName": "ABX Demo Test",            // <=  Name of the test
        "LoopByDefault": true,                  // <=  Enable looped playback by default
        "ShowFileIDs": false,                   // <=  Show file IDs for debugging (never enabled during real test!)
        "EnableABLoop": true,                   // <=  Show controls to loop playback with an AB range slider
        "EnableOnlineSubmission": false,        // <=  Enable transmission of JSON encoded results to a web service
        "BeaqleServiceURL": "",                 // <=  URL of the web service
        "SupervisorContact": "",                // <=  Email adress of supervisor to contact for help
        "RandomizeTestOrder": true,             // <=  Present test sets in a random order
        "MaxTestsPerRun": 3,                    // <=  
        "Testsets": [ {...}, {...}, ... ],      // <=  Definition of test sets and files, more details below
    }


### ABX ###

In an ABX test three items named A, B and X are presented to the listener, whereas X is randomly selected to be either the same as A or B. The listener has to identify which item is hidden behind X, or which one (A or B) is closest to X. If the listener is able to find the correct item, it reveals that there are perceptual differences between A and B. 

A typical application of ABX tests would be the evaluation of the transparency of audio codecs. For example item A could be an unencoded audio snippet and B is the same snippet but encoded with a lossy codec. When the listener is not able to identify if A or B was hidden in X (results are randomly distributed), one can assume that the audio coding was transparent
    
    ...                                         // <=  General options
    "Testsets": [
        { "Name": "Schubert",                   // <=  Name of the test set
          "Files": {                            // <=  Array with test files
            "A": "audio/schubert_ref.wav",      // <=  File A
            "B": "audio/schubert_2.wav",        // <=  File B
            }
        },
        { ... },                                // <=  Next test set starts here
        ....
    ]

### MUSHRA ###

In a MUSHRA test (ITU-R BS.1116-1) the listener gets presented an item marked as reference together with several anonymous test items. By using a slider for each test item he has to rate how close the items are to the reference on top. Among the test items there is usually also one hidden reference and one, or several, anchor signals to prove the validity of the ratings and the qualification of the participants.

Contrary to ABX tests the MUSHRA procedure allows more detailed evaluations as it is possible to compare more than one algorithm to a reference. Furthermore, the results are on a continuous scale allowing a direct numerical comparison of all algorithms under test.

    ...                                         // <=  General options
    "RateMinValue": 0,                          // <=  Minimum rating
    "RateMaxValue": 100,                        // <=  Maximum rating
    "RateDefaultValue":0,                       // <=  Default rating
    "Testsets": [
        { "Name": "Schubert 1",                 // <=  Name of the test set
          "TestID": "id1_1",                    // <=  Unique test set ID, necessary for internal referencing
          "Files": {                            // <=  Array with test files
            "Reference": "audio/ref.wav",       // <=  Every MUSHRA test set needs exactly one(!) file with a "Reference" label
            "label_1": "audio/algo_1.wav",      // <=  Various files to be tested, the labels can be freely chosen as desired
            "label_2": "audio/algo_2.wav",      //      but have to be unique inside a test set
            "label_3": "audio/algo_3.wav",      //      ...
            "anchor": "audio/algo_anch.wav",    //      ...
            }
        },
        { ... },                                // <=  Next test set starts here
        ....
    ]



Contact
---------------------

http://hsu-ant.github.io/beaqlejs

skraft (AT) hsu-hh.de


License
---------------------

The complete sources, html and script files as well as images are released unter the *GPLv3 
license*. A copy of the GPL is provided in the `LICENSE.txt` file.
