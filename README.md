# ProjectC
package com.example.pankaj.moneymanager.fragments;


import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;

import com.example.pankaj.moneymanager.R;

/**
 * A simple {@link Fragment} subclass.
 */
public class Simple extends Fragment {


    public Simple() {
        // Required empty public constructor
    }


    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_simple, container, false);
    }

}

---
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.example.pankaj.moneymanager.fragments.Simple">

    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerview"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

    </android.support.v7.widget.RecyclerView>


</RelativeLayout>
-----
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.example.pankaj.moneymanager.MainActivity">
    <FrameLayout
        android:id="@+id/main_frame"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

    </FrameLayout>

</FrameLayout>
----
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.example.pankaj.moneymanager.fragments.MoneyOptionFragment">

    <android.support.v7.widget.CardView
        android:id="@+id/income_card"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:contentPadding="30dp">

        <TextView
            android:id="@+id/income_id"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:gravity="center_horizontal"
            android:text="Income" />

    </android.support.v7.widget.CardView>

    <android.support.v7.widget.CardView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@+id/income_card"
        android:layout_marginTop="5dp"
        app:contentPadding="30dp">

        <TextView
            android:id="@+id/expense_id"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:gravity="center_horizontal"
            android:text="Expense" />
    </android.support.v7.widget.CardView>


</RelativeLayout>

----
package com.example.pankaj.moneymanager;

import android.app.Fragment;
import android.app.FragmentTransaction;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.TextView;
import android.widget.Toast;

import com.example.pankaj.moneymanager.fragments.MoneyOptionFragment;

public class MainActivity extends AppCompatActivity {


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        startFlow();
    }

    private void startFlow(){Fragment moneyOptionFragment=new MoneyOptionFragment();
        Fragment fragment=new MoneyOptionFragment();
        FragmentTransaction fragmentTransaction=getFragmentManager().beginTransaction();
        fragmentTransaction.addToBackStack(MoneyOptionFragment.TAG).replace(R.id.main_frame,fragment,MoneyOptionFragment.TAG).commit();
    }
}
-----
import java.math.BigDecimal;
import java.math.MathContext;

/**
 * Some shared methods are kept here
 */
abstract class AbstractMoney implements Money {
    /**
     * Add another Money object to this one.
     *
     * @param other Other Money object
     * @return A new Money object normalized to the efficient representation if possible
     */
    public Money add( final Money other ) {
        if ( other instanceof MoneyLong )
            return add( ( MoneyLong ) other );
        else
            return add( ( MoneyBigDecimal ) other );
    }

    protected abstract Money add( final MoneyLong other );

    protected Money add( final MoneyBigDecimal other )
    {
        final BigDecimal res = toBigDecimal().add( other.toBigDecimal(), MathContext.DECIMAL128 );
        return MoneyFactory.fromBigDecimal( res );
    }

    public boolean isZero() {
        return this.toDouble() == 0;
    }

    /**
     * Subtract another Money object from this one.
     *
     * @param other Other money object
     * @return A new Money object normalized to the efficient representation if possible
     */
    public Money subtract( final Money other )
    {
        return add( other.negate() );
    }

        public int compareTo(final Money other) {
        if (other instanceof MoneyLong) return compareTo((MoneyLong) other);
        else return compareTo((MoneyBigDecimal) other);
    }

    protected abstract int compareTo(final MoneyLong other);

    public int compareTo(final MoneyBigDecimal other) {
        return toBigDecimal().compareTo(other.toBigDecimal());
    }
}

------
package com.example.pankaj.moneymanager;
public class RequestAccountFragmentEvent {
    public RequestAccountFragmentEvent(int accountId) {
        this.accountId = accountId;
    }

    public int accountId;
}
-------------
public class UsernameLoadedEvent {
//    public UsernameLoadedEvent(String username) {
//        this.username = username;
//    }
//
//    public String username;
--------
mport android.content.Intent;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.text.TextUtils;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ImageButton;
import android.widget.TextView;


import com.mikepenz.google_material_typeface_library.GoogleMaterial;
import com.money.manager.ex.core.UIHelper;
import com.money.manager.ex.log.ErrorRaisedEvent;

import org.greenrobot.eventbus.EventBus;
import org.greenrobot.eventbus.Subscribe;

import butterknife.ButterKnife;
import butterknife.OnClick;
import timber.log.Timber;

public class PasscodeActivity
	extends AppCompatActivity {

	public static final String INTENT_REQUEST_PASSWORD = "com.money.manager.ex.custom.intent.action.REQUEST_PASSWORD";
	public static final String INTENT_MESSAGE_TEXT = "INTENT_MESSAGE_TEXT";
	public static final String INTENT_RESULT_PASSCODE = "INTENT_RESULT_PASSCODE";

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		// set theme
		try {
			UIHelper uiHelper = new UIHelper(getApplicationContext());
			setTheme(uiHelper.getThemeId());
		} catch (Exception e) {
			//Log.e(BaseListFragment.class.getSimpleName(), e.getMessage());
            Timber.e(e, "setting theme in passcode activity");
		}
		super.onCreate(savedInstanceState);
		setContentView(R.layout.passcode_activity);

        ButterKnife.bind(this);

        // create a listener for button
		OnClickListener clickListener = new OnClickListener() {
			@Override
			public void onClick(View v) {
				Button click = (Button) v;
				if (getWindow().getCurrentFocus() != null && getWindow().getCurrentFocus() instanceof EditText) {
					EditText getFocus = (EditText) getWindow().getCurrentFocus();
					if (getFocus != null && click.getTag() != null) {
						getFocus.setText(click.getTag().toString());
						//quick-fix convert 'switch' to 'if-else'
						if (getFocus.getId() == R.id.editTextPasscode1) {
							((EditText) findViewById(R.id.editTextPasscode2)).requestFocus();
						} else if (getFocus.getId() == R.id.editTextPasscode2) {
							((EditText) findViewById(R.id.editTextPasscode3)).requestFocus();
						} else if (getFocus.getId() == R.id.editTextPasscode3) {
							((EditText) findViewById(R.id.editTextPasscode4)).requestFocus();
						} else if (getFocus.getId() == R.id.editTextPasscode4) {
							((EditText) findViewById(R.id.editTextPasscode5)).requestFocus();
						} else if (getFocus.getId() == R.id.editTextPasscode5) {
							Intent result = new Intent();
							// set result
							result.putExtra(INTENT_RESULT_PASSCODE, ((EditText) findViewById(R.id.editTextPasscode1)).getText().toString()
									+ ((EditText) findViewById(R.id.editTextPasscode2)).getText().toString()
									+ ((EditText) findViewById(R.id.editTextPasscode3)).getText().toString()
									+ ((EditText) findViewById(R.id.editTextPasscode4)).getText().toString()
									+ ((EditText) findViewById(R.id.editTextPasscode5)).getText().toString());
							// return result
							setResult(RESULT_OK, result);
							finish();
						}
					}
				}
			}
		};
		// arrays of button id
		int ids[] = { R.id.buttonPasscode0, R.id.buttonPasscode1, R.id.buttonPasscode2,
			R.id.buttonPasscode3,
			R.id.buttonPasscode4, R.id.buttonPasscode5,
			R.id.buttonPasscode6, R.id.buttonPasscode7, R.id.buttonPasscode8, R.id.buttonPasscode9 };
		for (int i : ids) {
			Button button = (Button) findViewById(i);
			button.setOnClickListener(clickListener);
		}
		// textview message
		TextView textView = (TextView) findViewById(R.id.textViewMessage);
		textView.setText(null);
		// intent and action
		if (getIntent() != null && getIntent().getAction() != null) {
			if (INTENT_REQUEST_PASSWORD.equals(getIntent().getAction())) {
				if (getIntent().getStringExtra(INTENT_MESSAGE_TEXT) != null) {
					textView.setText(getIntent().getStringExtra(INTENT_MESSAGE_TEXT));
				}
			}
		}

        UIHelper ui = new UIHelper(this);
		ImageButton buttonKeyBack = (ImageButton) findViewById(R.id.buttonPasscodeKeyBack);
		buttonKeyBack.setImageDrawable(ui.getIcon(GoogleMaterial.Icon.gmd_backspace)
            .color(ui.getPrimaryTextColor()));
	}

	@Override
	protected void onStart() {
		super.onStart();

		EventBus.getDefault().register(this);
	}

	@Override
	protected void onStop() {
		EventBus.getDefault().unregister(this);

		super.onStop();
	}

	@Subscribe
	public void onEvent(ErrorRaisedEvent event) {
		// display the error to the user
		new UIHelper(this).showToast(event.message);
	}

	@OnClick(R.id.buttonPasscodeKeyBack)
	public void onBackspaceClick() {
		EditText getFocus = (EditText) getWindow().getCurrentFocus();
		if (getFocus != null) {
			boolean nullRequestFocus = false;
			if (!TextUtils.isEmpty(getFocus.getText())) {
				getFocus.setText(null);
			} else nullRequestFocus = true;
			//quick-fix convert 'switch' to 'if-else'
			if (getFocus.getId() == R.id.editTextPasscode1) {
			} else if (getFocus.getId() == R.id.editTextPasscode2) {
				((EditText) findViewById(R.id.editTextPasscode1)).requestFocus();
				if (nullRequestFocus) {
					((EditText) findViewById(R.id.editTextPasscode1)).setText(null);
				}
			} else if (getFocus.getId() == R.id.editTextPasscode3) {
				((EditText) findViewById(R.id.editTextPasscode2)).requestFocus();
				if (nullRequestFocus) {
					((EditText) findViewById(R.id.editTextPasscode2)).setText(null);
				}
			} else if (getFocus.getId() == R.id.editTextPasscode4) {
				((EditText) findViewById(R.id.editTextPasscode3)).requestFocus();
				if (nullRequestFocus) {
					((EditText) findViewById(R.id.editTextPasscode3)).setText(null);
				}
			} else if (getFocus.getId() == R.id.editTextPasscode5) {
				((EditText) findViewById(R.id.editTextPasscode4)).requestFocus();
				if (nullRequestFocus) {
					((EditText) findViewById(R.id.editTextPasscode4)).setText(null);
				}
			}
		}	}

}
---------
public enum AccountStatuses {
    OPEN ("Open"),
    CLOSED ("Closed");

    public static AccountStatuses get(String title) {
        for(AccountStatuses status : AccountStatuses.values()) {
            if (status.title.equals(title)) return status;
        }
        return null;
    }

    public final String title;

    private AccountStatuses(String s) {
        title = s;
    }

    public boolean equalsName(String otherName) {
        return (otherName == null) ? false : title.equalsIgnoreCase(otherName);
    }

    public String toString(){
        return title;
    }
}
-------

