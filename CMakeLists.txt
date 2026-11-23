# Version-aware dependencies for ESP-IDF 4 / 5 / 6
set(ESP_DMX_REQUIRES
  esp_timer
  esp_common
  esp_hw_support
  nvs_flash
)

if("${IDF_VERSION_MAJOR}" GREATER_EQUAL 6)
  # IDF 6: gptimer/uart/gpio are separate components; legacy timer removed
  list(APPEND ESP_DMX_REQUIRES
    esp_driver_gptimer
    esp_driver_uart
    esp_hal_uart
    esp_driver_gpio
    driver
  )
elseif("${IDF_VERSION_MAJOR}" GREATER_EQUAL 5)
  # IDF 5.x: gptimer exists as component; many UART/GPIO symbols still via driver
  list(APPEND ESP_DMX_REQUIRES
    driver
    esp_driver_gptimer
  )
else()
  # IDF 4.x: everything via legacy driver
  list(APPEND ESP_DMX_REQUIRES driver)
endif()

idf_component_register(
  SRCS
       # DMX driver HAL
       "src/dmx/hal/uart.c"
       "src/dmx/hal/timer.c"
       "src/dmx/hal/nvs.c"
       "src/dmx/hal/gpio.c"

       # DMX driver and sniffer
       "src/dmx/service.c"
       "src/dmx/driver.c"
       "src/dmx/io.c"
       "src/dmx/device.c"
       "src/dmx/parameter.c"
       "src/dmx/sniffer.c"

       # RDM driver
       "src/rdm/driver.c"

       # RDM controller
       "src/rdm/controller/discovery.c"
       "src/rdm/controller/product_info.c"
       "src/rdm/controller/device_control.c"
       "src/rdm/controller/dmx_setup.c"
       "src/rdm/controller/utils.c"

       # RDM responder
       "src/rdm/responder.c"
       "src/rdm/responder/discovery.c"
       "src/rdm/responder/product_info.c"
       "src/rdm/responder/rdm_info.c"
       "src/rdm/responder/device_control.c"
       "src/rdm/responder/queue_status.c"
       "src/rdm/responder/dmx_setup.c"
       "src/rdm/responder/sensor_parameter.c"
       "src/rdm/responder/power_lamp.c"
       "src/rdm/responder/utils.c"
  INCLUDE_DIRS "src"
  REQUIRES ${ESP_DMX_REQUIRES}
)
