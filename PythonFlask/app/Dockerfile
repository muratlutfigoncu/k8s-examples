FROM python:3.8-alpine

RUN mkdir /app
WORKDIR /app
ADD . /app/
RUN pip install Flask

EXPOSE 5000
CMD ["python", "/app/main.py"]
