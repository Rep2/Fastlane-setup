fastlane_version "2.53.1"

default_platform :ios

platform :ios do
    before_all do
	ensure_git_status_clean

	#carthage(platform: "iOS", cache_builds: true, use_submodules: true)
    end

    lane :production do
        increment_build_number

        match(type: "appstore", app_identifier: "com.")
        gym(scheme: "InitiativeTracker")

        pilot

	slack(
		message: "New app version available on Testflight",
		slack_url: ""
	)
    end

    lane :test do
        increment_build_number

        match(type: "adhoc", app_identifier: "com..test")
        gym(scheme: "- Test", configuration: "Test")

        crashlytics(
            api_token: "",
            build_secret: "",
            groups: ""
        )

    	version     = get_version_number(xcodeproj: ".xcodeproj")
    	build       = get_build_number(xcodeproj: ".xcodeproj")

    	slack(
      		message: "<!here|here>: New iOS *#{version}* (#{build}) :rocket:",
		slack_url: ""
	)
    end

  lane :devices do
    register_devices(devices_file: "./fastlane/devices.txt")

    match(type: "appstore", app_identifier: "com.", force_for_new_devices: true)
    match(type: "adhoc", app_identifier: "com.", force_for_new_devices: true)
    match(type: "adhoc", app_identifier: "com..staging", force_for_new_devices: true)
    match(type: "adhoc", app_identifier: "com..test", force_for_new_devices: true)
    match(type: "development", app_identifier: "com..debug", force_for_new_devices: true)
  end

end

