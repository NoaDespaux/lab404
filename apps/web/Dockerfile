FROM dhi.io/python:3.14.10-debian13-dev@sha256:490a2607efe2d8234b16da40c41fbca6d12799f046bf0def488ce868fc5b43da AS build

WORKDIR /src
RUN python3 -m venv --without-pip /opt/venv
COPY requirements.txt .
RUN python3 -m pip --python /opt/venv/bin/python install --no-cache-dir \
    --require-hashes --only-binary=:all: -r requirements.txt

FROM dhi.io/python:3.14.10-debian13@sha256:238eb9b564c3eec6de43cdaddfee6d92f946ae38a367d763c5e2fec4472a4814

ENV PATH="/opt/venv/bin:$PATH"

WORKDIR /app
COPY --from=build /opt/venv /opt/venv
COPY app.py ./
COPY templates ./templates

USER 65532:65532
EXPOSE 8080
CMD ["opentelemetry-instrument", "gunicorn", "--bind=0.0.0.0:8080", "--access-logfile=-", "app:app"]
