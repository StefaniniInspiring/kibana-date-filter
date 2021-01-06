<script>
  import { onMount } from 'svelte'
  import Litepicker from 'litepicker'
  import 'litepicker-module-ranges'
  import moment from 'moment'

  let dateRange
  let picker
  let src
  let defaultSrc

  onMount(async () => {
    document.addEventListener('DOMContentLoaded', function () {
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
          ranges: { Hoje: [toDayBeginnig(new Date()), toDayEnd(new Date())] },
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
    })
  })

  function toDayBeginnig(date) {
    date.setHours(0)
    date.setMinutes(0)
    date.setSeconds(0)
    return date
  }

  function toDayEnd(date) {
    date.setHours(23,59,59,999);
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

    src = encodeURI(src.replace(/time:\(.*?\)\)/g, newString))

    return src
  }

  hideFilters()

  src = findUrlParam('url')
  defaultSrc = src

</script>

<style>
  iframe {
    width: 100%;
    height: calc(100% - 34px);
    border: none;
  }

  div {
    display: flex;
    justify-content: center;
  }

  input {
    height: 24px;
    width: 200px;
    text-align: center;
  }
</style>

{#if src}
  <div>
    <input type="text" id="litepicker" placeholder="Alterar intervalo" />
  </div>
  <iframe title="none" {src} />
{:else}Ops, url do dashboard não encontrada{/if}
