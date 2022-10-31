# PlexDBRepair

## Introduction

DBRepair provides database repair and maintenance for the most common  Plex Media Server database problems.
It is a simple menu-driven utility with a command line backend.
## Situations and errors commonly seen include:

        1. Database is malformed
        2. Corruption
        3. Damaged indexes
        4. Database bloat after optimization

## Functions provided

        1.  Check the databases
        2.  Vacuum the databases
        3.  Reindex the databases
        4.  Repair damaged databases
        5.  Restore databases from most recent backup
        6.  Undo (undo last operation)
        7.  Show logfile of past actions and status

## Hosts currently supported

        1. ASUSTOR
        2. Netgear (OS5 Linux-based systems)
        3. Linux workstation & server
        4. QNAP (QTS & QuTS)
        5. Synology (DSM 6 & DSM 7)
        6. Docker (via 'docker exec' command line)
        7. Western Digital (OS5)

 ## How to install

    Where to place the utility varies from host to host.
    Please use this table as a reference.

```
    Vendor             | Shared folder name  |  directory
    -------------------+---------------------+------------------------------------------
    ASUSTOR            | Public              |  /volume1/Public
    Netgear (ReadyNAS) | "your_choice"       |  "/data/your_choice"
    Synology (DSM 6)   | Plex                |  /volume1/Plex             (change volume as required)
    Synology (DSM 7)   | PlexMediaServer     |  /volume1/PlexMediaServer  (change volume as required)
    QNAP (QTS/QuTS)    | Public              |  /share/Public
    Western Digital    | Public              |  /mnt/HD/HD_a2/Public      (Does not support 'MyCloudHome' series)
    Docker             | N/A                 |  Container root (adjacent /config)
    Linux (wkstn/svr)  | N/A                 |  Anywhere
```
###    To launch on NAS or Workstation:
        1. Place the tar/zip/sh file in the appropriate directory
        2. Open a command line session (usually SSH)
        3. Elevate privilege level to root
        4. Extract the utility from the tar or zip file
        4. Invoke the utility

    (Synology DSM 6 used as example)
```
        cd /volume1/Plex
        sudo bash
        tar xf DBRepair.tar
        chmod +x DBRepair.sh
        ./DBRepair.sh
```

###    To launch in a container:
```
        sudo docker exec -it plex /bin/bash
        /plex_service.sh -d   # Stop Plex
        tar xf DBRepair.tar
        chmod +x DBRepair.sh
        ./DBRepair.sh
```
###    To launch from the command line
```
        sudo bash
        systemctl stop plexmediaserver
        cd /path/to/DBRepair.tar
        tar xf DBRepair.tar
        ./DBRepair.sh
```


## The menu

  Plex Media Server Database Repair Utility


    Select

      1. Check database
      2. Vacuum database
      3. Reindex database
      4. Attempt database repair
      5. Replace current database with newest usable backup copy
      6. Undo last successful action (Vacuum, Reindex, Repair, or Replace)
      7. Show logfile
      8. Exit

Enter choice:

## Typical usage

  This utility can only operate on PMS when PMS is in the stopped state.
  If PMS is running when you startup the utility,  it will tell you.

   A. Database is malformed  (Backups of  com.plexapp.plugins.library.db and com.plexap.plugins.library.blobs.db  available)
      1. Check   - (Option 1) - Confirm either main or blobs database is damaged
      2. Replace - (Option 5) - Use the most recent valid backup -- OR -- Option 4 (Repair).  Look at the date/time for best action.
         -- If Replace fails, use Repair (Option 4) -  Replace can fail if the database has been damaged for a long time.
      3. Reindex - (Option 3) - Generate new indexes so PMS doesn't need to at startup
      4. Exit    - (Option 8)

   B. Database is malformed - No Backups
      1. Check   - (Option 1) - Confirm either main or blobs database is damaged
      2. Repair  - (Option 4) -  Salavage as much data as possible from the databases and rebuild them into a usable database.
      3. Reindex - (Option 3) - Generate new indexes so PMS doesn't need to at startup
      4. Exit    - (Option 8)

   C. Database sizes excessively large when compared to amount of media indexed (item count)
      1. Check   - (Option 1) - Make certain both databases are fully intact  (repair if needed)
      2. Vacuum  - (Option 2) - Instruct SQLite to rebuild its tables and recover unused space.
      3. Reinex  - (Option 3) - Rebuild Indexes.
      4. Exit    - (Option 8)

   D. Undo
      Undo is a special case where you need the utility to backup ONE step.
      This is rarely needed.  The only time you might want/need to backup one step.   It can only undo ONE step.

  Special considerations:

    1. As stated above, this utilty requires PMS to be stopped in order to do what it does.
    2. *TRICK* - This utility CAN sit at the menu prompt with PMS running.
       - You did a few things and want to check BEFORE exiting the utility
       - If you don't like how it worked out,
        -- STOP PMS
        -- UNDO the last action and do something else
        -- OR do more things to the databases
    3. When satisfied,  Exit the utility.
       - There is no harm in keeping the database temp files (except for space used)
       - ALL database temps are named with date-time stamps in the name to avoid confusion.
    4.The Logfile (Option 7) shows all actions performed WITH the timestamp so you can locate intermediate databases if desired
      for special / manual recovery cases.


## Exiting

  When exiting,  you will be asked whether to keep the interim temp files created during this session.
  If you've encountered any difficulties or aren't sure what to do,  don't delete them.
  You'll be able to ask in the Plex forums about what to do.  Be prepared to present the log file to them.


