# syntax=docker/dockerfile:1
FROM golang as base
WORKDIR /go/src/supabase
# Edelweiss: use our fork of AppFlowy's auth (GoTrue) for OIDC customizations
ARG GOTRUE_REPO=https://github.com/JonasMht/edelweiss-gotrue.git
ARG GOTRUE_BRANCH=master
RUN git clone "$GOTRUE_REPO" auth --depth 1 --branch "$GOTRUE_BRANCH"
WORKDIR /go/src/supabase/auth
RUN CGO_ENABLED=0 go build -o /auth .

FROM alpine:3.20
RUN adduser -D -u 1000 supabase

RUN apk add --no-cache ca-certificates curl
USER supabase

COPY --from=base /auth .
COPY --from=base /go/src/supabase/auth/migrations ./migrations

COPY start.sh .
CMD ["./start.sh"]
