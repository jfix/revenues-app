<script>
    import { Col, Card, Badge, Popover } from "sveltestrap"
    export let header, data, legend, color = 'info'
</script>

<Col xs="12" md="6" lg="6" class="mb-4">
    <Card class="text-white bg-{color}">
        <div class="card-header">{header}
            <Badge id="help2" pill href="#" color="light" class="ml-1">?</Badge>
            <Popover placement="top" target="help2" trigger="hover">
                <div>
                    The displayed amount below has been rounded up to the nearest Euro.
                    The precise amount is <strong>{data}</strong> €.
                </div>
            </Popover>
        </div>
        <div class="card-body">
            <h4 class="card-title text-center percentage-header">{`${Math.round(parseFloat(data)).toLocaleString()} €`}</h4>
          <p class="text-right card-text">{legend}</p>
        </div>
    </Card>
</Col>

<style>
    .percentage-header {
        font-size: 300%;
        font-weight: bold;
    }
</style>