<template>
  <v-col cols="12" md="6" class="DataCard">
    <positive-rate-mixed-chart
      :title="$t('検査実施人数（陰性確認を除く）と陽性率の推移')"
      :title-id="'positive-rate'"
      :chart-id="'positive-rate-chart'"
      :chart-data="positiveRateGraph"
      :date="PositiveRate.date"
      :items="positiveRateItems"
      :labels="positiveRateLabels"
      :unit="$t('人')"
      :data-labels="positiveRateDataLabels"
      :table-labels="positiveRateTableLabels"
    >
      <template v-slot:description>
        <ul :class="$style.GraphDesc">
          <li>
            {{
              $t(
                '（注）陽性率：陽性判明数の移動平均／（陽性判明数＋陰性判明数）の移動平均'
              )
            }}
          </li>
          <li>
            {{
              $t(
                '（注）集団感染発生や曜日による件数のばらつきにより、日々の結果が変動するため、こうしたばらつきを平準化し全体の傾向を見る趣旨から、過去7日間の移動平均値を陽性率として算出（例えば、5月7日の陽性率は、5月1日から5月7日までの実績平均を用いて算出）'
              )
            }}
          </li>
          <li>
            {{ $t('（注）検査結果の判明日を基準とする。') }}
          </li>
          <li>
            {{
              $t(
                '（注）5月7日以降は（１）東京都健康安全研究センター、（２）PCRセンター（地域外来・検査センター）、（３）医療機関での保険適用検査実績により算出。4月10日～5月6日は（３）が含まれず（１）（２）のみ、4月9日以前は（２）（３）が含まれず（１）のみのデータ'
              )
            }}
          </li>
          <li>
            {{
              $t(
                '（注）陽性者が1月24日、25日、30日、2月13日にそれぞれ1名、2月14日に2名発生しているが、有意な数値がとれる2月15日から作成'
              )
            }}
          </li>
          <li>
            {{
              $t(
                '（注）速報値として公開するものであり、後日確定データとして修正される場合あり'
              )
            }}
          </li>
        </ul>
      </template>
    </positive-rate-mixed-chart>
  </v-col>
</template>

<script>
import dayjs from 'dayjs'
import duration from 'dayjs/plugin/duration'
import PositiveRate from '@/data/positive_rate.json'
import PositiveRateMixedChart from '@/components/PositiveRateMixedChart'
dayjs.extend(duration)

export default {
  components: {
    PositiveRateMixedChart
  },
  data() {
    // 検査実施日別状況
    const l = PositiveRate.data.length
    const positiveCount = []
    const negativeCount = []
    const positiveRates = []
    const positiveRateLabels = []
    for (let i = 0; i < l; i++) {
      positiveCount.push(PositiveRate.data[i].positive_count)
      positiveRates.push(PositiveRate.data[i].positive_rate)
      negativeCount.push(PositiveRate.data[i].negative_count)
      positiveRateLabels.push(PositiveRate.data[i].diagnosed_date)
    }

    const positiveRateGraph = [positiveCount, negativeCount, positiveRates]
    const positiveRateItems = [
      this.$t('陽性者数'),
      this.$t('陰性者数'),
      this.$t('陽性率')
    ]
    const positiveRateDataLabels = [
      this.$t('陽性者数'),
      this.$t('陰性者数'),
      this.$t('陽性率')
    ]
    const positiveRateTableLabels = [
      this.$t('陽性者数'),
      this.$t('陰性者数'),
      this.$t('検査実施人数'),
      this.$t('陽性率')
    ]

    const data = {
      PositiveRate,
      positiveRateGraph,
      positiveRateItems,
      positiveRateLabels,
      positiveRateDataLabels,
      positiveRateTableLabels
    }
    return data
  }
}
</script>

<style module lang="scss">
.Graph {
  &Desc {
    margin: 0;
    margin-top: 1rem;
    padding-left: 0 !important;
    color: $gray-3;
    list-style: none;
    @include font-size(12);
  }
}
</style>
