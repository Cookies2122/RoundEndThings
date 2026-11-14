<h1 align=center><code>RoundEndThings</code></h1>

<div align=center>
  <img src=https://img.shields.io/badge/Counter--Strike%202-Metamod%202.0%20(KHook)-orange?style=flat-square alt=metamod>
  <a href=https://github.com/Cookies2122/RoundEndThings/commits><img src=https://img.shields.io/github/last-commit/Cookies2122/RoundEndThings?style=flat-square alt="last commit"></a>
  <a href=LICENSE><img src=https://img.shields.io/github/license/Cookies2122/RoundEndThings?style=flat-square alt=license></a>
</div>
<br>

В конце раунда включает игрокам ускорение, низкую гравитацию и распрыжку. Каждый игрок может отключить это себе через меню.

> Автор плагина: [ABKAM](https://github.com/ABKAM2023). Здесь версия 1.0.1 под новый SDK и Metamod 2.0, оригинал: [ABKAM2023/RoundEndThings](https://github.com/ABKAM2023/RoundEndThings).

## Что нужно

- [Metamod:Source](https://github.com/alliedmodders/metamod-source) 2.0 (KHook)
- [Utils](https://github.com/Pisex/cs2-menus)

## Установка

Соберите плагин (раздел [Сборка](#сборка)) и скопируйте содержимое `build/package` в папку `csgo` на сервере. В пакете лежит:

| Файл | Куда |
| --- | --- |
| `RoundEndThings.so` | `addons/RoundEndThings/` |
| `RoundEndThings.vdf` | `addons/metamod/` |
| `settings.ini` | `addons/configs/RoundEndThings/` |

После этого перезапустите сервер или смените карту.

## Команды

| Команда | Что делает |
| --- | --- |
| `!ret` | меню, где можно отключить ускорение и гравитацию для себя (в консоли `mm_ret`) |

## Настройки

### settings.ini

`addons/configs/RoundEndThings/settings.ini`

| Ключ | По умолчанию | Что делает |
| --- | --- | --- |
| `bhop_enabled` | `1` | распрыжка в конце раунда, 1 включить, 0 выключить |
| `speed_enabled` | `1` | ускорение в конце раунда, 1 включить, 0 выключить |
| `gravity_enabled` | `1` | другая гравитация в конце раунда, 1 включить, 0 выключить |
| `speed` | `2` | скорость: 1.0 обычная, 2.0 в два раза быстрее |
| `gravity` | `0.5` | гравитация: 1.0 обычная, 0.5 в два раза ниже, прыжок выше |

```
"Config"
{
    "bhop_enabled" "1"   // Включить/отключить BHOP (1 - включено, 0 - отключено)
    "speed_enabled" "1"   // Включить/отключить скорость (1 - включено, 0 - отключено)
    "gravity_enabled" "1"   // Включить/отключить гравитацию (1 - включено, 0 - отключено)
    "speed" "2"   // Скорость в конце раунда (1.0 - стандартная | 2.0 - в 2 раза быстрее)
    "gravity" "0.5"   // Гравитация в конце раунда (1.0 - стандартная | 0.5 - в 2 раза ниже (то есть выше прыжок))
}
```

## Что изменено

По сравнению с оригиналом:

- Переведено на новый SDK ([Wend4r/sourcesdk](https://github.com/Wend4r/sourcesdk)), старый после обновления CS2 от 12.11.2026 уже не подходит.
- Работает только на Metamod 2.0 (KHook), старый Metamod на SourceHook больше не поддерживается.
- Заголовки Utils обновлены под текущую версию, имена интерфейсов теперь `UTILS_INTERFACE` и `MENUS_INTERFACE`.
- Своя функция `GameEntitySystem()` убрана, в новом SDK entity system берётся через `IGameResourceService`.
- SchemaEntity обновлена: актуальные оффсеты виртуальных функций.
- При выгрузке плагин проверяет, что Utils ещё загружен. Раньше `meta unload` после выгрузки Utils мог уронить сервер.
- Меню открывается новым вызовом `DisplayPlayerMenu` из Utils.

## Сборка

Собирается на Linux. Нужны clang, python3, cmake и [AMBuild](https://github.com/alliedmodders/ambuild). Все команды ниже выполняются из одной папки.

**1. SDK.** Нужен [sourcesdk](https://github.com/Wend4r/sourcesdk) не старее обновления CS2 от 12.11.2026. Собирается один раз:

```bash
git clone --recursive https://github.com/Wend4r/sourcesdk hl2sdk-cs2
cmake -S hl2sdk-cs2 -B hl2sdk-cs2/build -DCMAKE_BUILD_TYPE=Release -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ -DSOURCESDK_AM_DEFINES=ON -DSOURCESDK_USE_MINIMAL_PROTOS=OFF
cmake --build hl2sdk-cs2/build -j$(nproc)
export HL2SDKCS2=$PWD/hl2sdk-cs2
```

**2. Metamod.** Нужен Metamod 2.0 с KHook, ветка `master`. Со старым Metamod на SourceHook сборка остановится с ошибкой.

```bash
git clone --recursive https://github.com/alliedmodders/metamod-source
```

**3. Плагин.** Папка SchemaEntity должна лежать рядом с папкой плагина.

```bash
git clone https://github.com/{o}/SchemaEntity
git clone https://github.com/Cookies2122/RoundEndThings
cd RoundEndThings
mkdir build && cd build
python3 ../configure.py --enable-optimize --sdks=cs2 --targets=x86_64 --mms_path=$PWD/../../metamod-source --hl2sdk-manifests=../hl2sdk-manifests
ambuild
```

Готовые файлы появятся в `build/package`.

## Автор

Плагин написал [ABKAM](https://github.com/ABKAM2023), оригинал: [ABKAM2023/RoundEndThings](https://github.com/ABKAM2023/RoundEndThings).
Перенос на новый SDK и Metamod 2.0, исправления: \_ded\_cookies.

## Лицензия

GPL-3.0, текст в файле [LICENSE](LICENSE). В коде плагина лицензия тоже указана как GPL. Авторские права на оригинальный код у ABKAM.
