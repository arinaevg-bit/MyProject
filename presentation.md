---
marp: true
theme: uncover
paginate: true
---

# Лабораторная работа №11
Калькулятор и презентация


---

## Калькулятор
            if operation == '+':
                result = num1 + num2
            elif operation == '-':
                result = num1 - num2
            elif operation == '*':
                result = num1 * num2
            elif operation == '/':
                if num2 == 0:
                    error = "На ноль делить нельзя!" 
                else:
                    result = num1 / num2

операции в views.py

---

## Калькулятор

    from django.db import models

    class CalculationHistory(models.Model):
        num1 = models.FloatField()
        num2 = models.FloatField()
        operation = models.CharField(max_length=1)
        result = models.FloatField()
        created_at = models.DateTimeField(auto_now_add=True) # Автоматически ставить время

        def __str__(self):
                return f"{self.num1} {self.operation} {self.num2} = {self.result}"

Файл models.py



---

## Калькулятор

    <div class="history-block">
        <h4>История (последние 5):</h4>
        {% for item in history %}
            <div class="history-item">
                {{ item.num1 }} {{ item.operation }} {{ item.num2 }} = <b>{{ item.result }}</b>
                <br>
                <small style="color: #999;">{{ item.created_at|date:"H:i:s" }}</small>
            </div>
        {% empty %}
            <p style="color: gray;">История пуста</p>
        {% endfor %}

Вид истории запросов в index.html


---

##  Калькулятор

![alt text](image-2.png)
Результат


---

## Docker

        FROM python:3.14-slim
        COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/
        WORKDIR /app
        COPY pyproject.toml .
        RUN uv sync --no-dev
        COPY . .
        ENV PATH="/app/.venv/bin:$PATH"
        EXPOSE 8000
        CMD ["sh", "-c", "python manage.py makemigrations && python manage.py migrate && python manage.py runserver 0.0.0.0:8000"]

Содержание Dockerfile

---

## Docker

    services:
        web:
            build: .
            ports:
                - "8000:8000"
            volumes:
                - ./db.sqlite3:/app/db.sqlite3
            command: >
                sh -c "python manage.py migrate &&
                            sh -c "python manage.py runserver 0.0.0.0:8000"

Содержание docker-compose

---

## Docker

![width:700px](image-4.png)
Результат

---

## Презентация Marp

![width:600px](image-5.png)

Результат