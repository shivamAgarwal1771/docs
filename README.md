import { LightningElement } from 'lwc';
import INTERACTION_HISTORY from '@salesforce/resourceUrl/Interation_history';
import CHART_IMAGE from '@salesforce/resourceUrl/chartImage';
import { loadScript } from 'lightning/platformResourceLoader';
import ChartJS from '@salesforce/resourceUrl/AWC_Chat';

export default class SimulationRisk extends LightningElement {
    chartJsInitialized = false;
    icon = INTERACTION_HISTORY;
    chart = CHART_IMAGE;
    chartInstance;

    renderedCallback() {
        if (this.chartJsInitialized) {
            return;
        }
        this.chartJsInitialized = true;

        Promise.all([loadScript(this, ChartJS)])
            .then(() => {
                this.initializeChart();
                window.addEventListener('resize', this.resizeHandler.bind(this));
            })
            .catch(error => {
                console.error('Error loading ChartJS', error);
            });
    }

    disconnectedCallback() {
        window.removeEventListener('resize', this.resizeHandler.bind(this));
    }

    initializeChart() {
        const ctx = this.template.querySelector('canvas.gauge-chart').getContext('2d');
        const data = {
            datasets: [{
                data: [750, 250],
                backgroundColor: ['#ff6384', '#e7e9ed'],
                borderWidth: 0,
            }]
        };

        const options = {
            responsive: true,
            maintainAspectRatio: false,
            rotation: 1 * Math.PI,
            circumference: 1 * Math.PI,
            cutoutPercentage: 80,
            tooltips: { enabled: false },
            hover: { mode: null }
        };

        this.chartInstance = new Chart(ctx, {
            type: 'doughnut',
            data: data,
            options: options
        });
    }

    resizeHandler() {
        if (this.chartInstance) {
            this.chartInstance.resize();
        }
    }
}
