# DRAM_Device_Type
check the DRAM Device Type of memory
/** @file
  This sample application bases on HelloWorld PCD setting 
  to print "UEFI Hello World!" to the UEFI Console.

  Copyright (c) 2006 - 2008, Intel Corporation. All rights reserved.<BR>
  This program and the accompanying materials                          
  are licensed and made available under the terms and conditions of the BSD License         
  which accompanies this distribution.  The full text of the license may be found at        
  http://opensource.org/licenses/bsd-license.php                                            

  THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN "AS IS" BASIS,                     
  WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS OR IMPLIED.             

**/

#include <Uefi.h>
//#include <Library/PcdLib.h>
#include <Library/UefiLib.h>
//#include <Library/UefiApplicationEntryPoint.h>
#include <Library/IoLib.h>

/**
  The user Entry Point for Application. The user code starts with this function
  as the real entry point for the application.

  @param[in] ImageHandle    The firmware allocated handle for the EFI image.  
  @param[in] SystemTable    A pointer to the EFI System Table.
  
  @retval EFI_SUCCESS       The entry point is executed successfully.
  @retval other             Some error occurs when executing this entry point.

**/
EFI_STATUS
EFIAPI
UefiMain (
  IN EFI_HANDLE        ImageHandle,
  IN EFI_SYSTEM_TABLE  *SystemTable
  )
{
    IoWrite8(0xF040, 0x1E); //将HST_STS(00H)设置为1EH,清掉各标志位
    IoWrite8(0xF044, 0xA1); //将XMIT_SLVA(04H)设置为A1H，说明是要读取SPD
    IoWrite8(0xF043, 0x02); //将HST_CMD(03H)设置为02H,说明读取SPD中的02H处的数据
    IoWrite8(0xF042, 0x48); //将HST_CNT(02H)设置为48H(采用字节读取方式，并START)，这是可以看到HST_CNT(02h)变成了08H，HST_STS（00h）变成了42H
    while(IoRead8(0xF040) != 0x42){ //因为各器件的速率不一样，等待HST_STS（00h）变成42H

    }
    Print(L"The DRAM Device Type of memory is %2X\n",IoRead8(0xF045)); //从0xF045读取1 Byte data

    return EFI_SUCCESS;
}
