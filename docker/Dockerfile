# Imagen para Coolify: incluye el repo en /workspace para que init.sh
# funcione aunque Coolify reemplace el bind mount por un volumen nombrado.
# Construir desde la raíz del repo: docker build -f docker/Dockerfile -t lms-frappe .

FROM frappe/bench:latest
COPY . /workspace
