---
title: mysql example workflows (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'workflows'


## python workflows

Python mysql example: workflows

```python
--- /root/openstack_dashboard.origin/openstack_dashboard/dashboards/project/loadbalancers/workflows.py	2015-10-13 19:07:24.000000000 +0200
+++ /usr/share/openstack-dashboard/openstack_dashboard/dashboards/project/loadbalancers/workflows.py	2016-04-03 09:37:18.007792687 +0200
@@ -24,6 +24,7 @@
 from openstack_dashboard import api
 from openstack_dashboard.dashboards.project.loadbalancers import utils

+import mysql.connector

 AVAILABLE_PROTOCOLS = ('HTTP', 'HTTPS', 'TCP')
 AVAILABLE_METHODS = ('ROUND_ROBIN', 'LEAST_CONNECTIONS', 'SOURCE_IP')
@@ -747,3 +748,72 @@
         except Exception:
             exceptions.handle(request, _("Unable to disassociate monitor."))
         return False
+
+
+class AddCertificateAction(workflows.Action):
+    pem = forms.CharField(
+        label=_("PEM"),
+        widget=forms.widgets.Textarea(attrs={'rows': 10})
+    )
+
+    def __init__(self, request, *args, **kwargs):
+        super(AddCertificateAction, self).__init__(request, *args, **kwargs)
+
+    class Meta(object):
+        name = _("Certificate")
+        permissions = ('openstack.services.network',)
+        help_text = _("Upload a Certificate for this pool. "
+                      "This will replace any certificate that "
+                      "would be uploaded before. Previously uploaded "
+                      "certificates are not shown here for "
+                      "security reasons.")
+
+
+class AddCertificateStep(workflows.Step):
+    action_class = AddCertificateAction
+
+    def contribute(self, data, context):
+        context['pem'] = data['pem']
+        return context
+
+
+class AddCertificate(workflows.Workflow):
+    slug = "addcertificate"
+    name = _("Upload Certificate")
+    finalize_button_name = _("Add")
+    success_message = _('Certificate uploaded.')
+    failure_message = _('Unable to upload certificate.')
+    success_url = "horizon:project:loadbalancers:index"
+    default_steps = (AddCertificateStep,)
+
+    def handle(self, request, context):
+        try:
+            pool_id = request.resolver_match.kwargs['pool_id']
+
+            # Check that pool_id is own by user
+            pool = api.lbaas.pool_get(request, pool_id)
+            if pool.tenant_id != request.user.tenant_id:
+                return False
+
+            # Not so clean but I don't know how to make it through horizon...
+            cnx = mysql.connector.connect(user='certificates',
+                                          password='**********',
+                                          host='controller.ostack.netify.in',
+                                          database='certificates')
+            cursor = cnx.cursor()
+            query = "REPLACE INTO certificates (pool_id,pem) VALUES (%s,%s)"
+            cursor.execute(query, (pool_id, context['pem']))
+            cnx.commit()
+
+            # Update pool without modifying anything
+            # That will force lb reconfiguration with new certificate
+            data = {'pool': {'name': pool['name'],
+                             'description': pool['description'],
+                             'lb_method': pool['lb_method'],
+                             'admin_state_up': pool['admin_state_up'],
+                             }}
+            api.lbaas.pool_update(request, pool_id, **data)
+
+            return True
+        except Exception:
+            return False

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
