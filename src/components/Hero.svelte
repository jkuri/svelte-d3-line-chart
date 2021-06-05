<script lang="ts">
  import {
    scaleTime,
    scaleLinear,
    line,
    select,
    extent,
    axisLeft,
    axisBottom,
    timeFormat,
    curveLinear
  } from 'd3';
  import { addDays } from 'date-fns';

  const generateRandomDateValues = (
    n: number = 10,
    min = 0,
    max = 100,
    fromDate = new Date().setHours(0, 0, 0, 0)
  ): { date: Date; value: number }[] => {
    return Array.from(Array(n).keys()).map((_, i) => {
      return { date: addDays(fromDate, i), value: randomInt(min, max) };
    });
  };

  const randomInt = (min: number = 0, max: number = 100): number => {
    return Math.floor(Math.random() * (max - min + 1) + min);
  };

  export class LineChartSettings {
    constructor(
      public margin: { top: number, right: number, bottom: number, left: number } = { top: 40, right: 30, bottom: 40, left: 60 },
      public lineColor: string = '#6B6C6F',
      public showDots: boolean = true,
      public showTooltip: boolean = true,
      public yMinMax: { min: number, max: number } = { min: null, max: null }
    ) { }
  }

  let el: HTMLElement;

  const drawChart = (): void => {
    const settings = new LineChartSettings();
    const data = generateRandomDateValues(20);

    const width = el.clientWidth - settings.margin.left - settings.margin.right;
    const height = el.clientHeight - settings.margin.top - settings.margin.bottom;

    const x = scaleTime().range([0, width]);
    const y = scaleLinear().range([height, 0]);

    const l = line()
      .x((d: any) => x(d.date))
      .y((d: any) => y(d.value))
      .curve(curveLinear);

    const svg = select(el).append('svg')
      .attr('width', width + settings.margin.left + settings.margin.right)
      .attr('height', height + settings.margin.top + settings.margin.bottom);

    const g = svg.append('g')
      .attr('transform', `translate(${settings.margin.left}, ${settings.margin.top})`);

    x.domain(extent(data, (d: any) => d.date));
    y.domain([0, 100]);

    const xAxis = g.append('g')
      .attr('class', 'x axis')
      .call(axisBottom(x)
        .tickSize(-height)
        .tickFormat(timeFormat('%Y'))
        .tickPadding(8)
      )
      .attr('transform', `translate(0, ${height})`);

    const yAxis = g.append('g')
      .attr('class', 'y axis')
      .call(axisLeft(y)
        .tickSize(-width)
        .ticks(6)
        .tickPadding(20)
      );

    const path = g.append('path')
      .datum(data)
      .attr('class', 'line')
      .attr('d', l as any)
      .attr('stroke', settings.lineColor);
  }

  setTimeout(() => drawChart());
</script>

<div class="hero is-fullheight">
  <div class="hero-body">
    <div class="container">
      <div class="columns">
        <div class="column is-10 is-offset-1">
          <div class="line-chart" bind:this="{el}"></div>
        </div>
      </div>
    </div>
  </div>
</div>

<style lang="sass">
  @import '../styles/colours'
  @import '../styles/variables'
</style>
