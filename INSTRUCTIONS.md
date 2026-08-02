# Порядок вызова хуков в шаблоне:

1. pre_fetch() — до скачивания исходников. Редко используется.
2. do_fetch() — скачивает архивы из distfiles. Обычно берётся из build_style, переопределяют редко.
3. post_fetch() — после скачивания, до распаковки.
4. pre_extract() — перед распаковкой архива.
5. do_extract() — распаковывает архив в $wrksrc. Обычно из build_style.
6. post_extract() — после распаковки, но до патчей.
7. pre_patch() — перед применением патчей из папки patches/.
8. do_patch() — применяет патчи. Обычно из build_style.
9. post_patch() — после патчей, перед настройкой. Тут часто правят файлы через sed, как у тебя с Hyprland.
10. pre_configure() — перед настройкой проекта (configure/cmake).
11. do_configure() — запускает ./configure, cmake и т. п. Обычно из build_style на основе configure_args.
12. post_configure() — после настройки, перед сборкой.
13. pre_build() — перед компиляцией. Тут можно, например, подправить Makefile или CMake-кэш.
14. do_build() — компилирует проект (make, cargo build, ninja). Обычно из build_style.
15. post_build() — после компиляции, перед тестами.
16. pre_check() — перед запуском тестов.
17. do_check() — запускает тесты (make check, ctest, cargo test). По умолчанию часто отключён.
18. post_check() — после тестов, перед установкой.
19. pre_install() — перед установкой файлов в ${DESTDIR}.
20. do_install() — устанавливает файлы в ${DESTDIR} (через make install, cargo install и т. д.). Обычно из build_style.
21. post_install() — финальная доработка содержимого пакета: создание скриптов (как тот run для runit), лицензий, удаление лишнего, копирование заголовков. Именно тут используют ${PKGDESTDIR} для новых файлов и ${DESTDIR} для доработки уже установленных.
22. do_clean() — очистка временных файлов.

----------------------------------
# Order of calling hooks in a template

1. pre_fetch() — runs before downloading sources. Rarely used.
2. do_fetch() — downloads archives from distfiles. Usually provided by build_style, rarely overridden.
3. post_fetch() — runs after downloading, before extraction.
4. pre_extract() — before extracting the archive.
5. do_extract() — extracts the archive into $wrksrc. Usually from build_style.
6. post_extract() — after extraction, before applying patches.
7. pre_patch() — before applying patches from the patches/ directory.
8. do_patch() — applies patches. Usually from build_style.
9. post_patch() — after patches, before configuration. Common place for sed edits (like your Hyprland fixes).
10. pre_configure() — before running the project’s configuration step (./configure, cmake, etc.).
11. do_configure() — runs the configuration command. Usually handled by build_style based on configure_args.
12. post_configure() — after configuration, before building.
13. pre_build() — before compilation. Good for tweaking Makefiles or CMake cache.
14. do_build() — compiles the project (make, cargo build, ninja, etc.). Usually from build_style.
15. post_build() — after compilation, before tests.
16. pre_check() — before running tests.
17. do_check() — runs tests (make check, ctest, cargo test). Often disabled by default.
18. post_check() — after tests, before installation.
19. pre_install() — before installing files to ${DESTDIR}.
20. do_install() — installs files to ${DESTDIR} (via make install, cargo install, etc.). Usually from build_style.
21. post_install() — final touches for the package: creating runit scripts, copying licenses, removing unwanted files, installing headers/pkgconfig. This is where you use ${PKGDESTDIR} for new files and ${DESTDIR} to adjust already‑installed ones.
22. do_clean() — cleans up temporary files.
