Example Summary
---------------
The carrier wave (CW) example sends a continuous carrier wave or pseudo-random
modulated signal on a fixed frequency. The frequency and other RF settings can
be modified using SmartRF Studio.

Peripherals Exercised
---------------------
N/A

Resources & Jumper Settings
---------------------------
> If you're using an IDE (such as CCS or IAR), please refer to Board.html in your project
directory for resources used and board-specific jumper settings. Otherwise, you can find
Board.html in the directory &lt;SDK_INSTALL_DIR&gt;/source/ti/boards/&lt;BOARD&gt;.

Board Specific Settings
-----------------------
1. The default frequency is: 
    - 433.92 MHz for the CC1350-LAUNCHXL-433
    - 433.92 MHz for the CC1352P-4-LAUNCHXL
    - 2440 MHz on the CC2640R2-LAUNXHL 
    - 868.0 MHz for other launchpads
In order to change frequency, modify the smartrf_settings.c file. This can be 
done using the code export feature in Smart RF Studio, or directly in the file
2. On the CC1352P1 the high PA is enabled (high output power) for all 
Sub-1 GHz modes by default. The user must set USE_SUB1_HIGH_PA_SETTING to 0 in 
smartrf_settings.h and smartrf_settings_predefined.h, and rebuild the example, 
to disable the high PA and use the default PA
3. On the CC1352P-2 the high PA operation for Sub-1 GHz modes is not supported
4. On the CC1352P-4 the default PA is used for all Sub-1 GHz physical modes by 
default. The user must set USE_SUB1_HIGH_PA_SETTING to 1 in the 
smartrf_settings.h and smartrf_settings_predefined.h, and rebuild the example, 
to enable the high PA (high output power)
**CAUTION**  
    - This will change the center frequency for 2-GFSK to 490 MHz  
    - The center frequency for SimpleLink long range (SLR) stays at 433.92 MHz, 
    but the high output power violates the maximum power output requirement 
    for this band
5. The CC2640R2 is setup to run all proprietary physical modes at a center 
frequency of 2440 MHz, at a data rate of 250 Kbps

Example Usage
-------------
Run the example.

Application Design Details
--------------------------
This examples consists of a single task and the exported SmartRF Studio radio
settings.

To switch between carrier wave (1) and modulated signal (0) set the following
in the code (CW is set as default):

    RF_cmdTxTest.config.bUseCw = 1; // CW
    RF_cmdTxTest.config.bUseCw = 0; // modulated signal

In order to achieve +14 dBm output power, make sure that the define
CCFG_FORCE_VDDR_HH = 0x1 in ccfg.c. This requirement holds for CC13x2P boards
when using the default PA.

When the task is executed it:

1. Configures the radio for Proprietary mode
2. Explicitly configures CW (1) or Modulated (0). Default modulated mode is
   PRBS-15
3. Gets access to the radio via the RF drivers RF_open
4. Sets up the radio using CMD_PROP_RADIO_DIV_SETUP command
5. Sets the frequency using CMD_FS command
6. Sends the CMD_TX_TEST command to start sending the CW or Pseudo-random
   signal forever

Note for IAR users: When using the CC1310DK, the TI XDS110v3 USB Emulator must
be selected. For the CC1310_LAUNCHXL, select TI XDS110 Emulator. In both cases,
select the cJTAG interface.
