FROM debian

RUN apt update -y
RUN apt install -y python
RUN pip install emoji
COPY /home/aube/temp/python_app_build/app.py /app/app.py
ENTRYPOINT python3 /app/app.py