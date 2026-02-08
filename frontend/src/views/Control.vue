<template>
  <v-container fluid class="fill-height" align="center">
    <v-row justify="center" style="max-width: 1200px; width: 100%">
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
          <v-progress-circular :rotate="0" :size="200" :width="15">
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
          <v-color-picker :modes="['rgb', 'hex']" width="100%" />
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>
