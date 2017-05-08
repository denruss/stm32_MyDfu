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

3) ����� ������� ��������� �� �������� ���������, ���������� �������� � ������� RTC_BKP_DR1 �����, �������� �� ���� (�) � ������������� ��������������� (�).
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
