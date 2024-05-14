# Release Notes for the ASIST Admin-less Testbed 

## V3.1.0 Study-4 January 6
- Mod 3.7.2
  * Bomb defusal and remaining counts now shown in Minecraft overlay
  * Participants can vote to return to the store by pressing the "E" key
  * Improved messaging to player during the Shop phase
  * Rendering errors of armor and hunger bars corrected 


## V3.0.0 Study-4 January 1
- Store Interactions moved entirely to ClientMap
  * block_beacon_basic introduced as stand-in for communication beacons pending tool use final decisions
  * New Implemented Generalized Messages ( AsistMod 3.7.0 )
    * Event: UIClick --> MessageSpecs/Study4_Generalized_MessageSet/UIClick 

## V2.0.0 Study-4 December 6
- MVP Core Adminless Testbed system as demonstrated in the PI Meeting + SPSO && COMPLEX + VOLATILE BOMBS
  * ClientInstallationBundle now delivers all necessary packages and library without reaching out to the web
  * New Implemented Generalized Messages ( AsistMod 3.3.0 )
    * Event: ObjectStateChange --> MessageSpecs/Study4_Generalized_MessageSet/ObjectStateChange
    * Event: EnvironmentCreatedList --> MessageSpecs/Study4_Generalized_MessageSet/EnvironmentCreated/List
  * Auto Trial Export, First Look, and zip on Trial Stop
  * Survey ingestion structure created --> MessageSpecs/Study4_Generalized_MessageSet/SurveyIngestion
  
## V1.1.0 Study-4 November 16
- MVP Core Adminless Testbed system as demonstrated in the PI Meeting + SPSO && SIMPLE BOMBS MOD
  * SPSO system integrated into core testbed for now ( will be optionally run from a different machine in future releases)
  * Documentation updates in readme.md
  * Deprecation of old testbed scripts
  * Semi-functional ClientInstallationBundle ( bugs with Java Version, but works if you have Java installed in the standard way

## V1.0.0 Study-4 November 6

**Testbed**

- MVP Core Adminless Testbed system as demonstrated in the PI Meeting + SIMPLE BOMBS MOD
  * Players receive all 3 tools on mission start
  * Bombs can be placed anywhere on the map using the MapBlocks file @ Local/data/mods/MapBlocks_Orion_3D.csv
  * Incorrect Tool use results in bombs exploding
  * Correct tool use ( in sequence ) results in bombs disappearing
  * Associated Bomb messages not yet defined, but player state, Tool Use and stage transition messages functioning
  * SPSO not quite complete - needs configurable IP settings - so continue to use procedure in docs/DemoTestbed.md for registering users

## V0.0.0 Study-4 October 19 PI Meeting

**Testbed**

- MVP Core Adminless Testbed system as demonstrated in the PI Meeting - 10/19/2022
  * Waiting Room with FIFO participant selection capabilities + placeholders for surveys and training documents
  * Client Map with sockets integrated for communication from Minecraft Server + placeholder for surveys + team dissolution protocol
  * Minecraft Mod v3.0.0 update with discrete space transitions from Shop to Field + various settings and configurations added
  * Demo Testbed documentation can be found in docs/DemoTestbed.md

