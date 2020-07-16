************
Bcome Guides
************

.. meta::
   :description lang=en: Bcome dev-ops application development framework guides


Quick test to see whether we can render ANSI trees in a nice way

      ▐▆   Ssh connection routes wbz
      │
      ├───╸ server
      │     namespace: wbz:gcp:gateways:bastion
      │     public ip address 104.155.101.98
      │     user guillaume
      │
      └───╸ proxy [1]
            bcome node wbz:gcp:gateways:bastion
            host 104.155.101.98
            user guillaume

                └───╸ proxy [2]
                      bcome node wbz:gcp:checkpoint:internal_jump
                      host 10.0.33.2
                      user guillaume

                          ├───╸ server
                          │     namespace: wbz:gcp:private:puppet
                          │     internal_ip_address 10.0.0.10
                          │     user guillaume
                          │
                          └───╸ server
                                namespace: wbz:gcp:private:wbzsite_app_sq6v
                                internal_ip_address 10.0.0.2
                                user guillaume

