<script>
    import { Card, Badge, Tooltip } from "sveltestrap";

    export let header,
        data,
        legend,
        color = "info";
</script>

<Card class="text-white bg-{color} mt-4">
    <div class="card-header">
        {header}
        <Badge id="help2" pill href="#" color="light" class="ml-1">?</Badge>
        <Tooltip placement="bottom" target="help2">
            <div>
                The displayed amount below has been rounded up to the nearest
                Euro. The precise amount is <strong>{data}</strong> €.
            </div>
        </Tooltip>
    </div>
    <div class="card-body">
        <h4 class="card-title text-center percentage-header">
            {`${Math.round(parseFloat(data)).toLocaleString()} €`}
        </h4>
        <p class="text-right card-text">{legend}</p>
    </div>
</Card>

<style>
    .percentage-header {
        font-size: 300%;
        font-weight: bold;
    }
</style>
