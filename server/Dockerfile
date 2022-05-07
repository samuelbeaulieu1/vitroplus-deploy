FROM surnet/alpine-wkhtmltopdf:3.15.0-0.12.6-full as wkhtmltopdf
FROM golang:1.18-alpine3.14

# Install dependencies for wkhtmltopdf
RUN apk add --no-cache \
  libstdc++ \
  libx11 \
  libxrender \
  libxext \
  libssl1.1 \
  ca-certificates \
  fontconfig \
  freetype \
  ttf-dejavu \
  ttf-droid \
  ttf-freefont \
  ttf-liberation \
&& apk add --no-cache --virtual .build-deps \
  msttcorefonts-installer \
\
# Install microsoft fonts
&& update-ms-fonts \
&& fc-cache -f \
\
# Clean up when done
&& rm -rf /tmp/* \
&& apk del .build-deps

# Copy wkhtmltopdf files from docker-wkhtmltopdf image
COPY --from=wkhtmltopdf /bin/wkhtmltopdf /bin/wkhtmltopdf


ENV DB_PASS
ENV DB_HOST mariadb
ENV DB_USER root
ENV DB vitroplus
ENV TOKEN_SECRET

RUN apk update && apk add git

WORKDIR /app

COPY app/go.mod app/go.sum ./

RUN go mod download && go mod verify

COPY app/ .

RUN go build -v -o server

EXPOSE 8000

ENTRYPOINT ["/app/server"]