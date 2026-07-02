# app_local — the "Local" LAN-source task app. Shares tasks.h via todo_common.
# WHOLE_ARCHIVE keeps the self-registration constructor.
idf_component_register(
    SRCS "app_local.c"
    REQUIRES taskmaster_core todo_common esp_http_client espressif__cjson
    WHOLE_ARCHIVE
)
