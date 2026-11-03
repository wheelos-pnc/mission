# WheelOS Mission

Independent `wheelos_mission` Bzlmod module. Source and Proto import paths
retain `modules/mission`. It owns mission lifecycle, behavior-tree nodes,
configuration and trees, and depends on shared Core/Common/message contracts,
BehaviorTree.CPP, TinyXML2 and SQLite, not the Apollo product.

From `/apollo`, in the mapped-user managed development container:

```bash
python3 scripts/bzlmod.py dev build --config=cpu --config=opt @wheelos_mission//modules/mission:libmission_component.so
python3 scripts/bzlmod.py dev test --config=cpu --config=opt @wheelos_mission//modules/mission:mission_lifecycle_test
```

`modules/mission:package_files` includes the plugin, configuration, launch
file and behavior trees. Source launch paths point to this checkout and the
Apollo Bazel outputs; actual mission execution still requires approved runtime
configuration.

Like `transform`, this ignored checkout must accompany the product changes.
Its Git source is registered in `wheelos.repos`; no Bzlmod registry release
has been published.
