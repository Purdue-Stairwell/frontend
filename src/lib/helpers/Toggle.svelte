<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();

    function update(event) {
        dispatch("stateChange", event.target.checked)
    }
</script>

<label class="switch">
    <input id="motion-toggle" type="checkbox" on:change={update}>
    <span class="slider round"></span>
</label>

<style>
    /* The switch - the box around the slider */
    .switch {
    position: relative;
    display: inline-block;
    width: 60px;
    height: 34px;
    }

    /* Hide default HTML checkbox */
    .switch input {
    opacity: 0;
    width: 0;
    height: 0;
    }

    /* The slider */
    .slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #ccc;
    -webkit-transition: .4s;
    transition: .4s;
    }

    .slider:before {
    position: absolute;
    content: "";
    height: 26px;
    width: 26px;
    left: 4px;
    bottom: 4px;
    background-color: white;
    -webkit-transition: .4s;
    transition: .4s;
    }

    input:checked + .slider {
    background-color: rgb(142, 111, 62);
    }

    input:focus + .slider {
    box-shadow: 0 0 1px rgb(142, 111, 62);
    }

    input:checked + .slider:before {
    -webkit-transform: translateX(23px);
    -ms-transform: translateX(23px);
    transform: translateX(23px);
    }
  </style>