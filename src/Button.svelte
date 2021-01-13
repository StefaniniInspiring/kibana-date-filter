<style>
  /*! CSS Used from: Embedded */
  *,
  *:before,
  *:after {
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
  }
  span {
    margin: 0;
    padding: 0;
    border: 0;
    font: inherit;
    font-family: inherit;
    vertical-align: baseline;
  }
  button {
    margin-left: 1em !important;
    font-family: inherit;
  }
  *:focus {
    outline: none;
  }
  button {
    cursor: pointer;
  }
  button {
    background: none;
    border: none;
    padding: 0;
    margin: 0;
    outline: none;
    font-size: 14px;
    color: inherit;
    font-size: inherit;
    border-radius: 0;
  }
  button:hover {
    cursor: pointer;
  }
  .euiButton {
    display: inline-block;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    cursor: pointer;
    height: 30px;
    line-height: 30px;
    text-decoration: none;
    border: solid 1px transparent;
    text-align: center;
    font-family: 'Open Sans', Helvetica, Arial, sans-serif;
    -webkit-transition: all 250ms cubic-bezier(0.34, 1.61, 0.7, 1);
    transition: all 250ms cubic-bezier(0.34, 1.61, 0.7, 1);
    white-space: nowrap;
    max-width: 100%;
    vertical-align: middle;
    -webkit-box-shadow: 0 2px 2px -1px rgba(153, 153, 153, 0.3);
    box-shadow: 0 2px 2px -1px rgba(153, 153, 153, 0.3);
    border-radius: 4px;
    min-width: 112px;
  }
  .euiButton:focus {
    text-decoration: underline;
  }
  .euiButton:focus {
    -webkit-animation: euiButtonActive 250ms cubic-bezier(0.34, 1.61, 0.7, 1);
    animation: euiButtonActive 250ms cubic-bezier(0.34, 1.61, 0.7, 1);
  }
  .euiButton .euiButton__content {
    height: 100%;
    width: 100%;
    vertical-align: middle;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    padding: 0 12px;
  }
  .euiButton .euiButton__text {
    text-overflow: ellipsis;
    overflow: hidden;
  }
  .euiButton:hover,
  .euiButton:active {
    -webkit-box-shadow: 0 4px 8px 0px rgba(153, 153, 153, 0.15),
      0 2px 2px -1px rgba(153, 153, 153, 0.3);
    box-shadow: 0 4px 8px 0px rgba(153, 153, 153, 0.15),
      0 2px 2px -1px rgba(153, 153, 153, 0.3);
  }
  .euiButton:enabled:hover,
  .euiButton:enabled:focus {
    background-color: rgba(0, 121, 165, 0.1);
  }
  .euiButton:disabled {
    color: silver;
    border-color: silver;
    pointer-events: none;
  }
  .euiButton:disabled .euiButton__content {
    pointer-events: auto;
    cursor: not-allowed;
  }
  .euiButton:disabled:hover,
  .euiButton:disabled:focus {
    -webkit-box-shadow: 0 2px 2px -1px rgba(153, 153, 153, 0.3);
    box-shadow: 0 2px 2px -1px rgba(153, 153, 153, 0.3);
    text-decoration: none;
  }
  .euiButton--primary {
    color: #0079a5;
    border-color: #0079a5;
  }
  .euiButton--primary:enabled {
    -webkit-box-shadow: 0 2px 2px -1px rgba(50, 98, 116, 0.3);
    box-shadow: 0 2px 2px -1px rgba(50, 98, 116, 0.3);
  }
  .euiButton--primary:enabled:hover,
  .euiButton--primary:enabled:focus {
    -webkit-box-shadow: 0 4px 8px 0px rgba(50, 98, 116, 0.15),
      0 2px 2px -1px rgba(50, 98, 116, 0.3);
    box-shadow: 0 4px 8px 0px rgba(50, 98, 116, 0.15),
      0 2px 2px -1px rgba(50, 98, 116, 0.3);
    background-color: rgba(0, 121, 165, 0.1);
  }
</style>

<button
  on:click
  class="euiButton euiButton--primary"
  type="button">
  <span class="euiButton__content">
    <span class="euiButton__text">Limpar filtro</span>
  </span>
</button>
