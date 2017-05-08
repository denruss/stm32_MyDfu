# ��� �������� ���������
��������� ��� ������� ���������:
1) CRC �������� ��������
2) ������� RTC_BKP_DR1 

���� CRC ��������� � ������� RTC_BKP_DR1 == 0, ����� ����������� �������� ���������.
���� �� CRC �� ��������� ��� RTC_BKP_DR1 != 0, ����� ��������� ��� ���� �������� *.dfu. 
��������� ��� ���� �������� ����� ���������� DfuSeDemo �� ��������� http://www.st.com/en/development-tools/stsw-stm32080.html 

![](https://habrastorage.org/web/6ab/74e/d1a/6ab74ed1a07c4cc5a98cbf6c307f8ba7.png)


# ��� ���� ������� � ������� �������� ���������
1) � ����� system_stm32f1xx.� ���� �������� #define VECT_TAB_OFFSET  0x00000000  -> 0x00006000 
2) � ���������� ����� (� ��������� IAR) ���� �������� ���������� CRC ����� � ����� ����� ��������

    ![](https://habrastorage.org/web/0d2/dc8/4c4/0d2dc84c47d34f648de50c353188e425.png)
    
3) � ������ ����� *.icf �������� ������ ��������� � �������� ������ checksum

/*###ICF### Section handled by ICF editor, don't touch! ****/

/*-Editor annotation file-*/

/* IcfEditorFile="$TOOLKIT_DIR$\config\ide\IcfEditor\cortex_v1_0.xml" */

/*-Specials-*/

define symbol __ICFEDIT_intvec_start__ = 0x08006000;

/*-Memory Regions-*/

define symbol __ICFEDIT_region_ROM_start__ = 0x08006000;

define symbol __ICFEDIT_region_ROM_end__   = 0x0800FBFF;

define symbol __ICFEDIT_region_RAM_start__ = 0x20000000;

define symbol __ICFEDIT_region_RAM_end__   = 0x20004FFF;

/*-Sizes-*/

define symbol __ICFEDIT_size_cstack__ = 0x800;

define symbol __ICFEDIT_size_heap__   = 0x800;

/**** End of ICF editor section. ###ICF###*/

define memory mem with size = 4G;

define region ROM_region   = mem:[from __ICFEDIT_region_ROM_start__   to __ICFEDIT_region_ROM_end__];

define region RAM_region   = mem:[from __ICFEDIT_region_RAM_start__   to __ICFEDIT_region_RAM_end__];

define block CSTACK    with alignment = 8, size = __ICFEDIT_size_cstack__   { };

define block HEAP      with alignment = 8, size = __ICFEDIT_size_heap__     { };

initialize by copy { readwrite };

do not initialize  { section .noinit };

place at address mem:__ICFEDIT_intvec_start__ { readonly section .intvec };

place in ROM_region   { readonly };

place in RAM_region   { readwrite,
                        block CSTACK, block HEAP };

/*place at address mem:__ICFEDIT_region_ROM_end__-3 { readonly section .checksum }; */

place at end of ROM_region { readonly section .checksum };

4) ����� ������� ��������� �� �������� ���������, ���������� �������� � ������� RTC_BKP_DR1 �����, �������� �� ���� (�) � ������������� ��������������� (�).
   
	 �) HAL_RTCEx_BKUPWrite(&RtcHandle, RTC_BKP_DR1, 1);

	 �) while(1) {} //���, ����� ��������� ���������� ������ � ������������ ��

�.�. �� ������ ��������� wdg � backup registers.    

# ��� ������� ���� ��� ��������, *.dfu

1) � ���������� ����� (� ��������� IAR) ���� �������� ��������� hex �����

    ![](https://habrastorage.org/web/084/588/3fd/0845883fdcde43a7be89ce06801ffab8.png)

2) �����, �� ����������� hex ����� ���� ������������� dfu ����, ��������� ��������� Dfuse File Maneger, �� ��������� http://www.st.com/en/development-tools/stsw-stm32080.html 

    ![](https://habrastorage.org/web/da1/cc7/a97/da1cc7a978564d878d5f7cb716b10d91.png)
    ![](https://habrastorage.org/web/fcc/d9d/42b/fccd9d42b5fa47649b73e35f4b1826b5.png)

# ������, ������� �������� � ���� �����������
https://github.com/denruss/usb_gen_v2_stm32

# �����
![](https://habrastorage.org/web/587/eab/d37/587eabd37f124b5c96ad89d1d5a8cbc1.png)
