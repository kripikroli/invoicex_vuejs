  
<template>
  <div class="page-dashboard">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title">Dashboard</h1>
      </div>

      <div class="column is-6">
        <div class="box">
          <h2 class="subtitle">Unpaid invoices</h2>

          <table class="table is-fullwidth">
            <thead>
              <tr>
                <th>#</th>
                <th>Client</th>
                <th>Amount</th>
                <th>Due date</th>
                <th></th>
              </tr>
            </thead>

            <tbody>
              <tr
                v-for="invoice in unpaidInvoices"
                v-bind:key="invoice.id"
              >
                <td>{{ invoice.invoice_number }}</td>
                <td>{{ invoice.client_name }}</td>
                <td>{{ invoice.gross_amount }}</td>
                <td>{{ invoice.get_due_date_formatted }}</td>
                <td>
                  <router-link :to="{ name: 'Invoice', params: { id: invoice.id }}">Details</router-link>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

      <div class="column is-6">
        <div class="box">
          <h2 class="subtitle">Newest clients</h2>

          <div
            v-for="client in limitedClients"
            v-bind:key="client.id"
          >
            <div class="box mb-2">
              <h3 class="is-size-4 mb-4">{{ client.name }}</h3>

              <router-link
                :to="{ name: 'Client', params: { id: client.id }}"
                class="button is-light"
              >Details</router-link>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";

export default {
  name: "Dashboard",
  methods: {
    ...mapActions("clients", ["getClients"]),
    ...mapActions("invoices", ["getInvoices"])
  },
  computed: {
    ...mapGetters("clients", ["limitedClients"]),
    ...mapGetters("invoices", ["invoices"]),
    unpaidInvoices() {
      return this.invoices.filter(invoice => invoice.is_paid === false);
    }
  },
  mounted() {
    this.getClients(), this.getInvoices();
  }
};
</script>