FROM python:3.8

WORKDIR /app

COPY ./requirements.txt ./
RUN python -m pip install --upgrade pip && pip install -r requirements.txt

EXPOSE 8000

COPY ./app.py ./app.py
COPY ./wsgi.py ./wsgi.py

CMD ["gunicorn", "--bind", "0.0.0.0:8000", "wsgi:app"]