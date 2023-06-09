# LVM менеджер логических томов

## Подсистема операционных систем Linux и OS/2, позволяющая использовать разные области одного жёсткого диска и/или области с разных жёстких дисков как один логический том

### Структура

sda1   sda2   sdv   sdc  
   |      |     |     |  
-----------------------  
         VG00  
-----------------------  
   |      |     |     |
 LV0    LV1    LV2   LV3  
   |      |     |     |
 ext3 reiserfs reiserfs xfs  

```s
apt install lvm2
```

### Плюсы

+ Логические тома не привязаны к физическому месторасположению
+ Их размер можно увеличить прмяо на лету
+ размонитрованные тома можно уменьшить во время работы системы
+ Тома можно распредить на нескольких физических дисках
+ Можно сделать снепшоты файловой системы и потом из них восстановиться

### Минусы

- Возможна дефрагментация
- Если выходит из строя диск можно получить битые данные

## Практика

Вывести информацию о группе томов

```a
svgdisplay
```

Смотрим VG Name

```s
svgdisplay vgname
```

Смотрим список подключенных устройств

```s
fdisk -l | less
```

