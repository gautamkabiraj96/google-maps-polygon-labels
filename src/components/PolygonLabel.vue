<template>
  <svg
    width="400"
    height="30"
    xmlns="http://www.w3.org/2000/svg"
    xmlns:svg="http://www.w3.org/2000/svg"
  >
    <g class="layer">
      <title>Polygon</title>
      <text
        style="
          font-family: Arial, Helvetica, sans-serif;
          font-weight: 600;
          text-shadow: -1px -1px 0 #ffffff, 1px -1px 0 #ffffff,
            -1px 1px 0 #ffffff, 1px 1px 0 #ffffff;
          font-size: 14px;
        "
        fill="#ffffff"
        class="pltext"
        text-anchor="middle"
        transform="matrix(0.9827 0 0 1 3.23004 0)"
        x="195.9824"
        xml:space="preserve"
        y="21"
      >
        {{ fieldName }}
      </text>
    </g>
  </svg>
</template>

<script>
export default {
  props: ["fieldName"],
};
</script>