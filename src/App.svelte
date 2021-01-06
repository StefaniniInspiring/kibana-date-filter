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
          ranges: { Hoje: [toDayBeginig(new Date()), toDayEnd(new Date())] },
        },
        mobileFriendly: true,
        autoApply: true,
        allowRepick: true,
        buttonText: {
          apply: 'OK',
          cancel: 'Cancelar',
        },
        onSelect: function (date1, date2) {
          picker.hide()
          console.log(changePeriod(date1, toDayEnd(date2)))
          console.log(date1, toDayEnd(date2))
        },
      })
    })
  })

  function toDayBeginig(date) {
    date.setHours(0)
    date.setMinutes(0)
    date.setSeconds(0)
    return date
  }

  function toDayEnd(date) {
    date.setHours(23)
    date.setMinutes(59)
    date.setSeconds(59)
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

    src = src.replace(/time:\(.*\)/, newString)

    return src
  }

  hideFilters()

  src = findUrlParam('url')
  defaultSrc = src;

  console.log(moment().toISOString())
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

