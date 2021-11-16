==================
pb.jsnapy.bgp.yaml
==================

----------------------
validate BGP neighbors
----------------------

jsnapy.bgp
==========

This module will validate the BGP neighbors on a JunOS device

Example
-------

Here is a basic example:

.. code-block:: yaml

    ### ---------------------------------------------------------------------------
    ### JSNAPY TESTS
    ### ---------------------------------------------------------------------------
    - hosts: all
      connection: local
      gather_facts: False
      become: False
      roles: 
        - juniper.junos
      tasks:

        # #################################################
        # ### Check BGP Neighbors                       ###
        # #################################################
        - name: "### CHECK BGP NEIGHBORS"
          juniper_junos_jsnapy:
            # define remote device parameters
            host: "{{ ansible_host }}"
            user: "automation"
            passwd: "juniper123"
  
            # define our test file and action
            test_files: tests/bgp.yaml
            action: snapcheck

          # store the output of our test in a new object test_bgp
          register: test_bgp

          # disable failed tests from cancelling the rest of our play
          ignore_errors: True

          # tag the result as test_bgp
          tags: [ test_bgp ]

        - name: "print test_bgp object to screen"
          debug:
            msg: "{{ test_bgp }}"
          # when:
          #   - "test_bgp.passPercentage != 100"

        - name: Check Alarms Test
          assert:
            that:
              - "test_bgp.passPercentage == 100"
          tags: [ test_bgp ]
