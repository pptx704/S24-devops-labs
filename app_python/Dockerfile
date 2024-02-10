FROM python:3.8-alpine

LABEL description="This is a simple flask app" maintainer="pptx704" 

WORKDIR /app

# Limited noroot user permission
RUN adduser -D newuser && mkdir /app/app_python && chown -R newuser ./app_python
USER newuser

# User path is added to run user installed packages
ENV PATH="/home/newuser/.local/bin:$PATH"

# Requiments is copied first to cache installation
COPY requirements.txt ./app_python/requirements.txt
RUN pip install -r ./app_python/requirements.txt --no-cache-dir --user

# Copy python files
COPY __init__.py app_utils.py app.py test.py ./app_python/

# Test the app
RUN python -m unittest app_python/test.py

# Bind gunicon to port 5000 with 4 workers
CMD ["gunicorn", "app_python.app:app", "-w 4", "-b 0.0.0.0:5000"]