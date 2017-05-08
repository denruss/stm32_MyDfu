# Как работает загрузчик
Загрузчик при запуске проверяет:
1) CRC основной прошивки
2) Регистр RTC_BKP_DR1 

Если CRC совпадает и регистр RTC_BKP_DR1 == 0, тогда запускается основная программа.
Если же CRC не совпадает или RTC_BKP_DR1 != 0, тогда загрузчик ждёт файл прошивки *.dfu. 
Отправить ему файл прошивки можно программой DfuSeDemo из комплекта http://www.st.com/en/development-tools/stsw-stm32080.html 

![](https://habrastorage.org/web/6ab/74e/d1a/6ab74ed1a07c4cc5a98cbf6c307f8ba7.png)


# Что надо сделать в проекте основной программы
1) В файле system_stm32f1xx.с надо изменить #define VECT_TAB_OFFSET  0x00000000  -> 0x00006000 
2) В настройках среды (я использую IAR) надо включить добавление CRC суммы в конце файла прошивки

    ![](https://habrastorage.org/web/0d2/dc8/4c4/0d2dc84c47d34f648de50c353188e425.png)

3) Чтобы вызвать загрузчик из основной программы, необходимо записать в регистр RTC_BKP_DR1 число, отличное от нуля (а) и перезагрузить микроконтроллер (б).
    а) HAL_RTCEx_BKUPWrite(&RtcHandle, RTC_BKP_DR1, 1);
    б) while(1) {} //ждём, когда сработает сторожевой таймер и перезагрузит МК
П.С. Не забыть настроить wdg и backup registers.    

# Как сделать файл для загрузки, *.dfu

1) В настройках среды (я использую IAR) надо включить генерацию hex файла

    ![](https://habrastorage.org/web/084/588/3fd/0845883fdcde43a7be89ce06801ffab8.png)

2) Далее, из полученного hex файла надо сгенерировать dfu файл, используя программу Dfuse File Maneger, из комплекта http://www.st.com/en/development-tools/stsw-stm32080.html 

    ![](https://habrastorage.org/web/da1/cc7/a97/da1cc7a978564d878d5f7cb716b10d91.png)
    ![](https://habrastorage.org/web/fcc/d9d/42b/fccd9d42b5fa47649b73e35f4b1826b5.png)

# Проект, который работает с этим загрузчиком
https://github.com/denruss/usb_gen_v2_stm32

# Схема
![](https://habrastorage.org/web/587/eab/d37/587eabd37f124b5c96ad89d1d5a8cbc1.png)
