# Judge Environment

- C (GCC 13)
- C++ (GCC 13)
- Java (Temurin 21)
- Python3 (CPython 3.12)
- Go (Go 1.22)
- JavaScript (Node.js 20)

## Customization

1. [Optional] Build custom judge server container image

   If you don't need to modify judge environment, you can skip this step.

   ```bash
   git clone https://github.com/QingdaoU/JudgeServer.git
   cd JudgeServer
   # modify something...
   docker buildx build . -t oj-image/judge --load
   ```

2. Download [languages.py](https://raw.githubusercontent.com/QingdaoU/OnlineJudge/master/judge/languages.py) to the deploy folder.

3. Edit and mount the languages.py

   ```yaml
   services:
     # ...
     oj-backend:
       # ...
       volumes:
         - ./data/backend:/data
         - ./languages.py:/app/judge/languages.py:ro
   ```

4. Start containers

   ```bash
   docker compose up -d
   ```

5. Overwrite judge language config in database from mounted languages.py

   ```bash
   docker compose exec -T oj-backend python manage.py shell <<EOF
   from options.options import SysOptions
   SysOptions.reset_languages()
   EOF
   ```
