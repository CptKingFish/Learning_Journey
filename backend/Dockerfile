FROM nikolaik/python-nodejs
# ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD true
USER root
RUN apt-get update && \
    apt-get -y install sudo

RUN sudo su
# RUN groupadd -g 1234 appuser && \
#     useradd -r -u 1234 -g appuser appuser
WORKDIR /app
RUN chmod -R 777 .

# Install Google Chrome Stable and fonts
# Note: this installs the necessary libs to make the browser work with Puppeteer.
RUN apt-get update && apt-get install curl gnupg -y \
    && curl --location --silent https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' \
    && apt-get update \
    && apt-get install google-chrome-stable -y --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*
RUN npm install -g @mermaid-js/mermaid-cli
RUN node /usr/lib/node_modules/@mermaid-js/mermaid-cli/node_modules/puppeteer/install.js 


# RUN chown -R appuser:root /app && \
#     chmod -R 775 /app
# ENV HOME=/app
# USER appuser

COPY ./requirements.txt /code/requirements.txt
RUN pip install --no-cache-dir --upgrade -r /code/requirements.txt
EXPOSE 8000
COPY . .
CMD ["uvicorn", "main:app", "--proxy-headers", "--host", "0.0.0.0", "--port", "8000"]