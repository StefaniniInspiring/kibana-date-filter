<script>
  import Button from './Button.svelte'
  import { onMount } from 'svelte'
  import Litepicker from 'litepicker'
  import 'litepicker-module-ranges'
  import moment from 'moment'

  let dateRange
  let picker
  let src
  let defaultSrc
  let showPicker = false
  let inputValue

  onMount(async () => {
    document.addEventListener('DOMContentLoaded', function () {
      if (src != null && (showPicker == 'true' || showPicker == true)) {
        picker = new Litepicker({
          element: document.getElementById('litepicker'),
          singleMode: false,
          format: 'DD/MM/YYYY',
          numberOfColumns: 2,
          numberOfMonths: 2,
          lang: 'pt-BR',
          tooltipText: {
            one: 'dia',
            other: 'dias',
          },
          useResetBtn: true,
          moveByOneMonth: true,
          moduleRanges: {
            position: 'left',
            ranges: {
              Hoje: [toDayBeginnig(new Date()), toDayEnd(new Date())],
              '7 dias': [
                toDayBeginnig(subDays(new Date(), 7)),
                toDayEnd(new Date()),
              ],
              '15 dias': [
                toDayBeginnig(subDays(new Date(), 15)),
                toDayEnd(new Date()),
              ],
              '30 dias': [
                toDayBeginnig(subDays(new Date(), 30)),
                toDayEnd(new Date()),
              ],
              '60 dias': [
                toDayBeginnig(subDays(new Date(), 60)),
                toDayEnd(new Date()),
              ],
              '90 dias': [
                toDayBeginnig(subDays(new Date(), 90)),
                toDayEnd(new Date()),
              ],
            },
          },
          mobileFriendly: true,
          autoApply: true,
          allowRepick: true,
          buttonText: {
            apply: 'OK',
            cancel: 'Cancelar',
            reset: `<span style="display: flex;">Limpar</span> <svg xmlns="http://www.w3.org/2000/svg" height="24" viewBox="0 0 24 24" width="24">
        <path d="M0 0h24v24H0z" fill="none"></path>
        <path d="M13 3c-4.97 0-9 4.03-9 9H1l3.89 3.89.07.14L9 12H6c0-3.87 3.13-7 7-7s7 3.13 7 7-3.13 7-7 7c-1.93 0-3.68-.79-4.94-2.06l-1.42 1.42C8.27 19.99 10.51 21 13 21c4.97 0 9-4.03 9-9s-4.03-9-9-9zm-1 5v5l4.28 2.54.72-1.21-3.5-2.08V8H12z"></path>
      </svg>`,
          },
          resetBtnCallback: function () {
            src = defaultSrc
            picker.hide()
          },
          onSelect: function (date1, date2) {
            picker.hide()
            changePeriod(date1, toDayEnd(date2))
          },
        })
      }
    })
  })

  function toDayBeginnig(date) {
    date.setHours(0)
    date.setMinutes(0)
    date.setSeconds(0)
    return date
  }

  function toDayEnd(date) {
    date.setHours(23, 59, 59, 999)
    return date
  }

  function hideFilters() {
    setInterval(() => {
      const iframes = document.getElementsByTagName('iframe')
      if (iframes != null && iframes.length > 0) {
        for (let i = 0; i < iframes.length; i++) {
          iframes[i]['contentWindow'].postMessage('removeKibanaFilters', '*')
        }
      }
    }, 1000)
  }

  function findUrlParam(paramName) {
    var result = null,
      tmp = []
    window.location.search
      .substr(1)
      .split('&')
      .forEach(function (item) {
        tmp = item.split('=')
        if (tmp[0] === paramName) {
          paramName == 'url'
            ? (result = decodeURIComponent(tmp[1]) + window.location.hash)
            : (result = decodeURIComponent(tmp[1]))
        }
      })
    return result
  }

  function changePeriod(iniDate, endDate) {
    let newString = `time:(from:'${moment(
      iniDate,
    ).toISOString()}',mode:absolute,to:'${moment(endDate).toISOString()}'))`

    src = src.replace(/time(:|%3A)\(.*?\)\)/g, newString)
    return src
  }

  function subDays(dateObj, numDays) {
    dateObj.setDate(dateObj.getDate() - numDays)
    return dateObj
  }

  function resetFilter() {
    src = defaultSrc
    picker.hide()
    inputValue = '';
  }

  hideFilters()

  src = findUrlParam('url')
  showPicker = findUrlParam('picker') ?? 'false'

  defaultSrc = src
</script>

<style>
  iframe {
    width: 100%;
    border: none;
    height: 100vh;
  }

  .heightfilter {
    height: calc(100% - 46px) !important;
  }

  /* .height-no-filter {
    height: 100vh;
  } */

  div {
    display: flex;
    align-items: center;
    background-color: white;
    border: solid 1px #d9d9d9;
    border-radius: 4px;
    margin: 0px 22px 4px 8px;
    padding: 5px;
    padding-left: 10px;
    font-size: 14px;
    color: #585656;
    box-shadow: 0 2px 2px -1px rgba(0, 0, 0, 0.1);
  }

  span {
    margin-right: 1em;
    color: #7b8a8b;
  }

  input {
    height: 24px;
    width: 200px;
    text-align: center;
    cursor: pointer;
  }
</style>

{#if src}
  {#if showPicker == 'true' || showPicker == true}
    <div>
      <span>Filtro data:</span>
      <input bind:value={inputValue} type="text" id="litepicker" placeholder="Alterar intervalo" />
      <Button on:click={resetFilter} />
    </div>
  {/if}
  <iframe class:heightfilter={showPicker === 'true'} title="none" {src} />
{:else}Ops, url do dashboard não encontrada{/if}
