# lab04
Вы продолжаете проходить стажировку в "Formatter Inc." 

В прошлый раз ваше задание заключалось в настройке автоматизированной системы CMake.

Сейчас вам требуется настроить систему непрерывной интеграции для библиотек и приложений, с которыми вы работали в прошлый раз. Настройте сборочные процедуры на различных платформах:

```
 git clone https://github.com/Wenir04/lab03.git
cd lab03
git remote remove origin
git remote add origin https://github.com/Wenir04/lab04
git push --set-upstream origin main
mkdir .github/workflows
cd .github/workflows
touch CI.yml
cat >> CI.yml<<EOF
> name: CMake

on:
 push:
  branches: [main]
 pull_request:
  branches: [main]

jobs: 
 build_Linux:

  runs-on: ubuntu-latest

  steps:
  - uses: actions/checkout@v3

  - name: Configure Solver
    run: cmake ${{github.workspace}}/solver_application/ -B ${{github.workspace}}/solver_application/build

  - name: Build Solver
    run: cmake --build ${{github.workspace}}/solver_application/build

  - name: Configure HelloWorld
    run: cmake ${{github.workspace}}/hello_world_application/ -B ${{github.works    run: cmake --build ${{github.workspace}}/hello_world_application/build.works
> EOF
bash: name: CMake
```
Отправляем коммит в наш репозиторий
```
git add .github
git commit -m "added CI.yml"
git push origin main
```
![изображение](https://github.com/Wenir04/laba04/assets/113133600/de634dcd-c52e-4922-8a86-8e65b4513be3)
