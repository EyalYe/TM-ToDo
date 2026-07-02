# app_cloud — the "Yapp" direct-Todoist task app. Shares tasks.h via todo_common.
# Needs esp-tls + mbedtls for the HTTPS cert bundle (Todoist over TLS).
idf_component_register(
    SRCS "app_cloud.c"
    REQUIRES taskmaster_core todo_common esp_http_client esp-tls mbedtls espressif__cjson
    WHOLE_ARCHIVE
)
