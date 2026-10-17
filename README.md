# Asenmod Virtual Audio Driver

Открытый виртуальный аудиодрайвер для Windows 10 и 11: виртуальный микрофон ("Asenmod Microphone") и виртуальный аудиовыход ("Asenmod Virtual Cable").

Драйвер используется приложением Asenmod (изменение голоса и войс-панель), но является самостоятельным открытым проектом и не содержит закрытого кода. Любой может использовать его отдельно.

## Происхождение и лицензии

Проект является форком [VirtualDrivers/Virtual-Audio-Driver](https://github.com/VirtualDrivers/Virtual-Audio-Driver) (лицензия MIT, автор MikeTheTech), который, в свою очередь, основан на примере Microsoft SysVAD из [Windows-driver-samples](https://github.com/microsoft/Windows-driver-samples) (лицензия MS-PL).

- `LICENSE` - лицензия MIT, текст сохранён без изменений.
- `THIRD_PARTY_NOTICES.md` - уведомления и текст MS-PL.

Изменения относительно исходного проекта:

- в `Source/Main/VirtualAudioDriver.inx` заменены издатель и отображаемые имена устройств (`Asenmod`, `Asenmod Microphone`, `Asenmod Virtual Cable`);
- идентификатор оборудования `ROOT\VirtualAudioDriver` и имена файлов драйвера не менялись.

## Состояние

- Исходные тексты скопированы из upstream, сборка в этом репозитории ещё не проверена.
- Передача звука из приложения в виртуальный микрофон пока не реализована: в исходном драйвере буфер записи заполняется нулями (`Source/Main/minwavertstream.cpp`). Планируется добавить кольцевой буфер в общей памяти, в который пишет приложение.

## Сборка

Требуются Visual Studio 2022 с рабочими нагрузками C++ и Windows Driver Kit (WDK) 11.

```
build.bat
```

Сборка в CI описана в `.github/workflows/VAD-compile.yml` (GitHub Actions, windows-latest).

## Установка для разработки

Драйвер без подписи Microsoft загружается только при включённом режиме тестовой подписи или при выключенном Secure Boot:

```
bcdedit /set testsigning on
```

Затем в диспетчере устройств: Действие -> Установить старое устройство -> Установка оборудования, выбранного из списка вручную -> Звуковые, игровые и видеоустройства -> Установить с диска -> `VirtualAudioDriver.inf`.

## Подпись кода

Релизные сборки планируется подписывать через [OSSign](https://ossign.org/). Раздел "Code signing policy" будет добавлен после одобрения.
