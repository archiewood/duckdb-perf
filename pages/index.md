# DuckDB Performance Benchmarks 

seconds, % improvement in performance over period

```sql benchmarks
select 
  substring(benchmark, 4, 99) as benchmark,
  benchmark_type,
  "Time (seconds)", 
  "Release Date",
  --window function to compare to first date pct
  -"Time (seconds)" / first_value("Time (seconds)") over (partition by benchmark order by "Release Date") +1.0 as improvement
from duckdb_perf.benchmark_results
where "Time (seconds)" != 0
and benchmark_type ilike '${inputs.type}' 
order by benchmark, "Release Date" desc
```

```sql benchmark_names
select 
  substring(benchmark, 4, 99) as benchmark,
  benchmark_type,
  sum("Time (seconds)") as "Total Time (seconds)"
from duckdb_perf.benchmark_results 
where "Time (seconds)" != 0
group by all
```

```sql benchmark_names_filtered
select * from ${benchmark_names} where benchmark_type ilike '${inputs.type}'
```




<ButtonGroup data={benchmark_names} name=type value=benchmark_type >
  <ButtonGroupItem value=% valueLabel="All Benchmarks" default/>
</ButtonGroup>


*{benchmark_names_filtered.length} / {benchmark_names.length} Benchmarks*

<Grid cols=5>

{#each benchmark_names_filtered as row}

{@const benchmark_rows = benchmarks.where(`benchmark = '${row.benchmark}'`)}

<div>
  <BigValue data={benchmark_rows} title={row.benchmark} value=improvement fmt=pct/>
  
  <AreaChart
    data={benchmark_rows}
    x="Release Date"
    y="Time (seconds)"
    yFmt=num
    yGridlines=false
    series="benchmark"
    chartAreaHeight=80
  />
</div>

{/each}

</Grid>