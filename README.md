# Docker for Data scientist

Rupyterlabs : 데이터 분석용 application (ML/AI 제외) (port : 8888)
DS_board : streamlit 기반 web application server (port : 8501)
DS_db : postgres 기반 db server (port : 5432)
DS_web : R-shiny 기반 r-shiny server (port : 3838)

## 1. Build, creates, starts
```
docker-compose --env-file=env.cfg up -d
```

## 2. Stops container
```
docker-compose --env-file=env.cfg down
```
