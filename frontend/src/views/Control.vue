<template>
  <v-container fluid class align="center">
    <v-row justify="center" style="max-width: 1200px">
      <v-col cols="12" md="6" align="center">
        <!--Sheet 1-->
        <v-sheet
          color="surface"
          :elevation="0"
          max-width="800"
          width="100%"
          class="mb-1 rounded-t-lg"
        >
          <v-card
            title="LED Controls"
            color="surface"
            subtitle="Recent settings"
            variant="tonal"
            flat
            class="text-secondary"
          />
        </v-sheet>

        <!--Sheet 2-->
        <v-sheet
          color="surface"
          :elevation="0"
          max-width="800"
          width="100%"
          class="mb-1"
        >
          <v-card color="surface" variant="tonal" class="pt-5">
            <v-slider
              append-icon="mdi:mdi-car-light-high"
              density="compact"
              :thumb-size="16"
              color="secondary"
              label="Brightness"
              direction="horizontal"
              :min="0"
              :max="250"
              :step="10"
              show-ticks
              thumb-label="always"
              class="pt-2 bg-surface"
              v-model="led.brightness"
            />
          </v-card>
        </v-sheet>

        <!--Sheet 3-->
        <v-sheet
          color="surface"
          :elevation="0"
          max-width="800"
          width="100%"
          class="mb-1"
          justify="center"
          align="center"
        >
          <v-card color="surface" variant="tonal" class="pt-5">
            <v-slider
              append-icon="mdi:mdi-led-on"
              density="compact"
              :thumb-size="16"
              color="secondary"
              label="LED Nodes"
              direction="horizontal"
              :min="1"
              :max="7"
              :step="1"
              show-ticks
              thumb-label="always"
              class="pt-2 bg-surface"
              v-model="led.leds"
            />
          </v-card>
        </v-sheet>

        <!--Sheet 4-->
        <v-sheet
          color="surface"
          :elevation="0"
          max-width="800"
          width="100%"
          class="pa-2 mb-1"
          justify="center"
          align="center"
          border
        >
          <v-progress-circular
            :rotate="0"
            :size="200"
            :width="15"
            :model-value="led.leds * 15"
            :color="indicatorColor"
          >
            <span class="text-onSurface font-weight-bold"
              >{{ led.leds }} LED(s)</span
            >
          </v-progress-circular>
        </v-sheet>
      </v-col>

      <v-col cols="12" md="6" align="center">
        <v-sheet
          color="surface"
          :elevation="0"
          max-width="800"
          width="100%"
          class="pa-4"
        >
          <v-color-picker width="100%" v-model="led.color" />
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { useMqttStore } from "@/store/mqttStore";
import { storeToRefs } from "pinia";
import { reactive, watch, computed, onMounted, onBeforeUnmount } from "vue";

//Variables
const Mqtt = useMqttStore();
const { payload, payloadTopic } = storeToRefs(Mqtt);

const led = reactive({
  brightness: 255,
  leds: 1,
  color: { r: 255, g: 0, b: 255, a: 1 },
});
let timer,
  ID = 1000;

// WATCHERS
watch(led, (controls) => {
  clearTimeout(ID);
  ID = setTimeout(() => {
    const message = JSON.stringify({
      type: "controls",
      brightness: controls.brightness,
      leds: controls.leds,
      color: controls.color,
    });
    Mqtt.publish("620172489_sub", message); // Publish to a topic subscribed to by the hardware
  }, 1000);
});

const indicatorColor = computed(() => {
  return `rgba(${led.color.r},${led.color.g},${led.color.b},${led.color.a})`;
});

onMounted(() => {
  // THIS FUNCTION IS CALLED AFTER THIS COMPONENT HAS BEEN MOUNTED
  Mqtt.connect(); // Connect to Broker located on the backend
  setTimeout(() => {
    // Subscribe to each topic
    Mqtt.subscribe("620172489");
  }, 3000);
});

onBeforeUnmount(() => {
  // THIS FUNCTION IS CALLED RIGHT BEFORE THIS COMPONENT IS UNMOUNTED
  // unsubscribe from all topics
  Mqtt.unsubscribeAll();
});
</script>
