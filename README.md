![Create new chip](https://github.com/JuliProg/SDTNQGAMA_008G/workflows/Create%20new%20chip/badge.svg?event=repository_dispatch)
![ChipUpdate](https://github.com/JuliProg/SDTNQGAMA_008G/workflows/ChipUpdate/badge.svg)
# Join the development of the project ([list of tasks](https://github.com/users/JuliProg/projects/1))


# SDTNQGAMA_008G
Implementation of the SDTNQGAMA_008G chip for the JuliProg programmer

Dependency injection, DI based on MEF framework is used to connect the chip to the programmer.

<section class = "listing">

# Chip parameters

```c#

        //--------!!!!!!!!!! WARNING   !!! . Chip not verified ---------------------------
        //--------------------Vendor Specific Pin configuration---------------------------

        //  VSP1(38pin) - NC    
        //  VSP2(35pin) - NC
        //  VSP3(20pin) - NC

        ChipAssembly()
        {
            myChip.devManuf = "SANDISK";
            myChip.name = "SDTNQGAMA_008G";
            myChip.chipID = "0645DE94937657";      // device ID - 06h,45h,DEh,94h,93h,76h,57h

            myChip.width = Organization.x8;    // chip width - 8 bit
            myChip.bytesPP = 16384;             // page size - 16384 byte (16Kb)
            myChip.spareBytesPP = 1280;          // size Spare Area - 1280 byte
            myChip.pagesPB = 256;               // the number of pages per block - 256 
            myChip.bloksPLUN = 2048;           // number of blocks in CE - 2048
            myChip.LUNs = 1;                   // the amount of CE in the chip
            myChip.colAdrCycles = 2;           // cycles for column addressing
            myChip.rowAdrCycles = 3;           // cycles for row addressing 
            myChip.vcc = Vcc.v3_3;             // supply voltage
            (myChip as ChipPrototype_v1).EccBits = 1;                // required Ecc bits for each 512 bytes

```
# Chip operations

```c#


            //------- Add chip operations    https://github.com/JuliProg/Wiki#command-set----------------------------------------------------

            myChip.Operations("Reset_FFh").                   // https://github.com/JuliProg/Wiki/wiki/Command-Sets#reset_ffhdll
                   Operations("Erase_60h_D0h").               // https://github.com/JuliProg/Wiki/wiki/Command-Sets#erase_60h_d0hdll
                   Operations("Read_00h_30h").                // https://github.com/JuliProg/Wiki/wiki/Command-Sets#read_00h_30hdll
                   Operations("PageProgram_80h_10h");         // https://github.com/JuliProg/Wiki/wiki/Command-Sets#pageprogram_80h_10hdll

```
# Chip registers (optional)

```c#


            //------- Add chip registers (optional)----------------------------------------------------

            myChip.registers.Add(                   // https://github.com/JuliProg/Wiki/wiki/StatusRegister
                "Status Register").
                Size(1).
                Operations("ReadStatus_70h").       // https://github.com/JuliProg/Wiki/wiki/Status-Register-operations#readstatus_70hdll
                Interpretation("SR_Interpreted").   // https://github.com/JuliProg/Wiki/wiki/Status-Register-Interpretation
                UseAsStatusRegister();



            myChip.registers.Add(                  // https://github.com/JuliProg/Wiki/wiki/ID-Register
                "Id Register").
                Size(7).
                Operations("ReadId_90h");          // https://github.com/JuliProg/Wiki/wiki/ID-Register-operations#readid_90hdll     
               // Interpretation(ID_interpreting);
            
           
           
            myChip.registers.Add(
                "Parameter Page").
                Size(768).
                Operations("ReadParameterPage_ECh");

```
# Interpretation of ID-register values ​​(optional)

```c#


        public string ID_interpreting(Register register)   
        
```
</section>





















