# todo_common — the shared task library used by BOTH the Local and Cloud apps.
#
# tasks.h is header-only (static-inline task view + offline cache + write queue), so
# this component just exposes the include directory; there's no .c to compile. Both
# apps REQUIRE it, so the shared logic lives in exactly one place. You choose which
# app to build in apps.yaml (they're alternative task sources — pick one).
idf_component_register(
    INCLUDE_DIRS "."
    REQUIRES taskmaster_core
)
