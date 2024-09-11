<template>
    <div class="tab-container">
        <lightning-tabset>
            <lightning-tab label="AI Assistant"  title="AI Assistant">
                <c-ai_assists></c-ai_assists>
            </lightning-tab>
            <template if:true={showCallContext}>
                <lightning-tab label="Call Context" title="Call Context">
                    Call Context Visible
                </lightning-tab>
            </template>
            <lightning-tab label="Call Summary" title="Call Summary">
                <c-awc_call-summary></c-awc_call-summary>
            </lightning-tab>
        </lightning-tabset>
    </div>
</template>
