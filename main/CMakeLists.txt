idf_component_register(SRCS "test_oled.cpp"
                    INCLUDE_DIRS "."
                    REQUIRES esp_adc driver esp_driver_i2c)
