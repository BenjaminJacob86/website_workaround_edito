# Dockerfile
#
# Exposes the SOCIB Marine Heatwaves page through port 8080.
#
# Build:
#   docker build -t socib-marine-heatwaves .
#
# Run:
#   docker run -d --name socib-marine-heatwaves -p 8080:8080 socib-marine-heatwaves
#
# Open:
#   http://localhost:8080

FROM nginx:alpine

RUN printf '%s\n' \
    'server {' \
    '    listen 8080;' \
    '    server_name _;' \
    '' \
    '    location / {' \
    '        return 302 https://www.socib.es/users/mjuza/subregmed-marineheatwaves-foccus/;' \
    '    }' \
    '}' \
    > /etc/nginx/conf.d/default.conf

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost:8080/ || exit 1