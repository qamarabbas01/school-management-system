<script>
    export let type = "button";
    export let disabled = false;
    export let onClick = () => {};
    export let className = "";
    export let mode = "primary"; // New prop for mode
  </script>
  
  <button
    type={type}
    disabled={disabled}
    on:click={onClick}
    class={`btn ${mode} ${className}`}
  >
    <slot></slot>
  </button>
  
  <style>
    .btn {
      padding: 0.5em 1em;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-size: 1em;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 0.5em;
    }
  
    .btn.primary {
      background-color: #007bff;
      color: white;
    }
  
    .btn.secondary {
      background-color: #6c757d;
      color: white;
    }
  
    .btn.success {
      background-color: #28a745;
      color: white;
    }
  
    .btn.danger {
      background-color: #dc3545;
      color: white;
    }
  
    .btn.warning {
      background-color: #ffc107;
      color: black;
    }
  
    .btn:disabled {
      background-color: #cccccc;
      cursor: not-allowed;
    }
  </style>