# Установка Minecraft Lyrics в разных дистрибутивах

Minecraft Lyrics — виджет для Serpantinum, а не отдельное приложение. Сначала нужна рабочая установка Serpantinum, затем сам виджет добавляется скриптом из этого проекта.

## 1. Поддерживаемые дистрибутивы

Официальный установщик Serpantinum поддерживает Arch Linux, Fedora и Void Linux с производными. Для NixOS предусмотрена отдельная установка через Flake и модули. Виджет Minecraft Lyrics рассчитан на Serpantinum v2.

### Arch Linux

Установка Serpantinum:

```bash
curl -fsSL install.serpantinum.dev | bash
```

После завершения установщика перезайди в Wayland-сеанс, если он попросит, и убедись, что Serpantinum запускается.

Поддерживаемые производные:

- CachyOS
- EndeavourOS
- Manjaro
- Garuda Linux
- Artix Linux

Для этих систем используется та же команда. Установщик определяет пакетный менеджер и init-систему самостоятельно.

### Fedora

```bash
curl -fsSL install.serpantinum.dev | bash
```

Также поддерживаются производные Fedora:

- Nobara
- Ultramarine

Установщик использует `dnf`. У Fedora есть отдельные особенности: например, пакет `gpu-screen-recorder` не доступен в стандартных репозиториях, поэтому соответствующая функция записи экрана может отсутствовать.

### Void Linux

```bash
curl -fsSL install.serpantinum.dev | bash
```

Поддерживаются варианты glibc и musl. Установка использует `xbps` и настраивает службы через runit. Обрати внимание на сообщения установщика: для Hyprland используется дополнительный репозиторий, а сетевые службы могут быть перенастроены.

### NixOS

Для NixOS не запускай общий shell-установщик. Используй Flake и модуль Serpantinum.

Добавь вход в `flake.nix`:

```nix
inputs.serpantinum.url = "github:ilyamiro/serpantinum";
```

Подключи модуль в конфигурации:

```nix
{ nixpkgs, serpantinum, ... }:
{
  # Внутри соответствующей конфигурации NixOS:
  programs.serpantinum.enable = true;
}
```

В `nixosSystem` передай `serpantinum` через `specialArgs` и добавь `serpantinum.nixosModules.default` в список модулей. Затем пересобери систему:

```bash
sudo nixos-rebuild switch --flake .
```

Если используешь Home Manager, подключи `serpantinum.homeManagerModules.default` и настрой пользовательский сервис по официальному руководству.

## 2. Установка самого виджета

Эти шаги одинаковы для всех перечисленных систем. Сначала установи и запусти Serpantinum v2.

Открой терминал в каталоге проекта Minecraft Lyrics и выполни:

```bash
python3 install.py
```

Скрипт копирует QML-файл и Minecraft-шрифт, добавляет компонент в реестр Serpantinum и перезагружает оболочку. Затем открой редактор виджетов и добавь **Minecraft Lyrics**.

Для работы текста песен необходим источник Lyrics, доступный внутри Serpantinum. Виджет сам не распознаёт музыку и не ищет тексты независимо от оболочки.

## 3. Популярные дистрибутивы без официальной поддержки Serpantinum

Следующие системы не перечислены в официальной инструкции Serpantinum как поддерживаемые установщиком:

- Ubuntu и Linux Mint
- Debian
- Pop!_OS и Zorin OS
- openSUSE Tumbleweed и Leap
- KDE neon

На них нельзя рекомендовать запускать официальный установщик наугад или подменять пакетный менеджер. Для этих систем потребуется отдельный порт Serpantinum с проверкой зависимостей, Wayland-композитора и служб. Пока такой порт не подготовлен, Minecraft Lyrics на них не считается поддерживаемым.

## 4. Композиторы

Serpantinum поддерживает:

- Hyprland
- Niri
- Sway
- Mango

Наличие одного из этих композиторов само по себе не заменяет Serpantinum: виджет использует его реестр компонентов и внутренний источник Lyrics.

## 5. Удаление виджета

В каталоге проекта выполни:

```bash
python3 uninstall.py
```

Скрипт удалит регистрацию и файлы Minecraft Lyrics. Сам Serpantinum и его настройки останутся.

## 6. Полезные ссылки

- [Официальная установка Serpantinum](https://serpantinum.dev/installation/)
- [NixOS: Flake](https://serpantinum.dev/installation/nixos)
- [NixOS: Home Manager](https://serpantinum.dev/installation/nixos/home-manager/)
- [Репозиторий Serpantinum](https://github.com/ilyamiro/serpantinum)

Перед установкой Serpantinum на рабочую систему прочитай официальную инструкцию целиком: установщик меняет компоненты Wayland-сеанса, службы и настройки входа в систему.
