FROM python:3.12-trixie AS builder
COPY poetry.lock pyproject.toml ./
ENV PATH=/root/.local/bin:$PATH
ENV POETRY_VERSION=1.8.5
RUN apt-get update \
  && apt-get install -y tzdata \
  && curl -sSL https://install.python-poetry.org | python3 - \
  && poetry config virtualenvs.create false \
  && poetry install --no-ansi --no-interaction

FROM python:3.12-slim-trixie
COPY --from=builder /usr/local/lib/python3.12/site-packages /usr/local/lib/python3.12/site-packages
COPY --from=builder /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
ENV PYTHONPATH=/usr/local/lib/python3.12/site-packages
ENV APP_HOME=/kot
WORKDIR $APP_HOME
COPY kot kot
COPY scripts scripts
# Turn off webdriver-manager logs use
ENV WDM_LOG=0

ENTRYPOINT ["python", "-m", "kot"]
