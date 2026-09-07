# Daily Journal
```
`05/07/26` -> - Designed prototype of Perf Board holder inside the avionics bay
              - Learned Shapr3d and Anycubic slicer  

`06/07/26` -> - Discared "Perf Holder Prot 0" and created and printed new design "Perf Holder Prot 1".
              - Disgned perf board in 49*20mm diamention for testing.
              - Discared "Perf Holder Prot 1" and designed new prototype "Perf Holder Prot 2".

`07/07/26` -> - Tried and FAILED in repairing my ESP32 module
              - Finalize which LoRa transceiver to use for both avoinics bay and ground station module

> Couldn't work on 8th and 9th july due to focus on acedemics

`10/07/26` -> - Designed Base of my avionics bay
              - Finalized desing of Perf board Holder
              - Posted journal on Macondo on "Optimizing component list"

> Major break due to...... IDK

`29/07/26` -> - Didnt did much after first shipping but they requested some changes.
              - Started writing my BOM.
              - Started writing my "Step to recreat" section in readme.

`31/07/26` -> - Completed BOM with md and pdf version.
              - Added .step files of all design and removed unnecesary iterations.

`02/08/26` -> - Started planning "Step to recreat" section in Readme.md.

`07/08/26` -> - Finalized BOM.md 
              - Finalized Readm.md

> Waited some time for parts to arrive

`31/08/26` -> - Wrote a test code to check health, physical wiring and bus address of sensors and I2C/SPI connection of sensors to MCU
              - Code showed my 9-axis MPU9250 does have a magnometer (AK8963) so i got worried but then i got to know that my module's AK8963 die is seperate from the SDA/SCL bus thus the code never got any signal from it so it showed that my module is just MPU6500.
              - I connected EDA and ECL which are pins for AK8963 and ran the test code and unfortunetly my module is just another MPU6500 labelled as MPU9250 also found that MPU9250 is discountinued resulting in these fake modules.
              - Decided to buy 7-semi's BNO085 , since 7-semi is a relable brand i get actual 9-DOF IMU

`01/09/26` -> -Requested more funds for BNO085 , wont be able to work untill it arrives.

> Waiting for $27 topup so i can buy the BNO085 IMU

>Small update "08/09/26", work have been paused due to funding delay, planning on shipping v1 with fake MPU9250 and optimize my code to compensate for the error due to lack of mango.
```