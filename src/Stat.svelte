<script>
    import { Col, Card } from "sveltestrap"
    export let header, data, legend, color = 'info'
    
</script>

<Col xs="12" md="6" lg="6" class="mb-4">
    <Card class="text-white bg-{color}">
        <div class="card-header">{header}</div>
        <div class="card-body">
            <h4 class="card-title text-center percentage-header">{data}</h4>
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